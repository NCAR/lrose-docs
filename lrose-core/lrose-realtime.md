# Real-time operations

## Overview

There are a number of components that make up the LROSE realtime system.

| Component | Description |
| -----     | ----------- |
| procmap   | Process mapper. This is a server. Individual applications register routinely with procmap to confirm that they are alive and running as expected. |
| apps      | Applications register with procmap |
| proc_list | List of required applications |
| procmap_list_start | Start script - reads proc_list, starts apps. (perl) |
| auto_restart | Auto restarter - reads proc_list, checks with procmap if all are running correctly. If a process is not registering correctly, it will kill it (in case it is hung) and restart it. (perl) |
| procmap_list_kill | Kill script - reads proc_list, kills apps. (perl) |
| cron | Keeps auto_restart running. Performs other tasks on a schedule as required |
| DataMapper | Data mapper. This is a server. Applications register with DataMapper when they write data. |
| start_all | Overall start script. (shell, generally c-shell) |
| stop_all | Overall stop script. (shell, generally c-shell) |
| print_procmap | App to print the procmap status. |
| PrintDataMap | App to print the DataMapper status. |

[The figure below shows how these components interact:](./lrose_runtime.png)

<img align="center" src="./lrose_runtime.png">

## ```procmap``` - the process mapper

The process mapper, ```procmap```, lies at the heart of the auto-restart capabilities of the lrose system. procmap keeps a table of the current status of all processes running on the system, except for itself. Each running process registers with procmap at regular intervals, usually once per minute. This is called the ‘heart-beat’ interval. The process status table is read from procmap by the auto_restart script and compared against the list of expected processes in the proc_list. If a process is missing or has not registered its heartbeat recently, it is killed (in case it is hung) and then restarted.

procmap may be queried by the application ```print_procmap``` which prints out the current table of processes, along with status information.

## ```DataMapper``` - the data mapper

The DataMapper performs a task similar to procmap, except for data sets instead of processes. The DataMapper keeps a table of all data sets on the system, along with such information as the last time data was added to the data set, how many files exist in the data set and how much disk space it occupies.

Each time an application writes data to disk it also registers that activity with the DataMapper. That allows the DataMapper to keep an up-to-date status table. The DataMapper table may be queried by the application ```PrintDataMap```, which then prints the table information.

## ```auto_restart``` - the auto restarter

This is the most important script. It is responsible for contacting procmap at regular intervals, say once per minute, and checking the table of processes which are running against the proc_list. Any processes which are missing or late in registering are killed with the kill script or kill_inst mechanism (in case they are hung) and then restarted with the start script.

## ```procmap_list_start``` - the process list start script

This script is used at system startup to go through the proc_list and start all processes by calling the start scripts, or by using the ```start_inst``` mechanism.

## ```procmap_list_kill``` - the process list kill script

This script is called at system shutdown to go through the proc_list and kill all processes by calling the kill script or the ```kill_inst``` mechanism.


```
tdrp_gen -h
Usage:
  tdrp_gen [moduleName] [-h] -f paramdef_path
           [-c++] [-debug]
           [-class className] [-dir output_dir]
           [-prog progName] [-lib libName]
           [-singleton] [-add_ncar_copyright]

where:
  [moduleName] in C mode all externals are prepended
    with this name.
    moduleName must be first arg if it is specified.
    If first arg begins with -, moduleName is set
    to empty string.
  [-h] gives usage.
  [-f paramdef_path] parameter definition file path.
    This arg is REQUIRED.
  [-c++] C++ mode - generates .hh and .cc class files.
  [-debug] print debug messages.
  [-class className] In C++ mode, set the name of the params class.
    Default is 'Params'.
  [-dir path] optional dir path to which to write the output.
    Default is the current directory.
  [-prog progName] Program name for documenting code files.
  [-lib libName] Library name if the params reside in a library.
    This ensures the includes are set correctly.
  [-singleton] Create a singleton object. Only in C++ mode.
  [-add_ncar_copyright] Add NCAR copyright in C++ mode.

NOTES: TDRP - Table Driven Runtime Parameters.
  tdrp_gen performs code generation.
  tdrp_gen will generate two files, one header and one for code.
  In C mode, the default, it will generate the files:
    moduleName_tdrp.h and moduleName_tdrp.c.
  If moduleName is left out of the command line, the files will be:
    _tdrp.h and _tdrp.c.
  In C++ mode, it will generate the files:
    className.hh and classname.cc.
  If the -class arg is not specified, the files will be:
    Params.hh and Params.cc.
```

The Params class for TdrpTest is here:

| Source file | URL      |
| -------------     |:-------------:|
| Params.hh  | [Params.hh](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/tdrp/src/TdrpTest/Params.hh) |
| Params.cc  | [Params.cc](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/tdrp/src/TdrpTest/Params.cc) |

## Generating a parameter file.

Once an app is compiled, you can generate an example parameter file with the appropriate defaults, as follows (using Radx2Grid as an example):

```
  Radx2Grid -print_params > Radx2Grid.test
```

If you have a parameter file and you want to update it, you can use the following:

```
  Radx2Grid -params Radx2Grid.old_params -print_params > Radx2Grid.new_params
```

Just make sure .old_params and .new_params are not the same file, otherwise you will end up with an empty file.

## Runtime

At runtime, specify the parameter file on the command line:

```
  Radx2Grid -params Radx2Grid.good ... other args
```

## Code location

| TDRP code         | URL      |
| -------------     |:-------------:|
| lib       | https://github.com/NCAR/lrose-core/tree/master/codebase/libs/tdrp |
| apps      | https://github.com/NCAR/lrose-core/tree/master/codebase/apps/tdrp |

The most important TDRP app is `tdrp_gen`.

## Makefile support

In a standard Makefile, we handle tdrp using the following rules:

```
Params.hh: Params.cc

Params.cc: paramdef.appName
        tdrp_gen -f paramdef.appName -c++ -prog appName -add_ncar_copyright

clean_tdrp:
        /bin/rm Params.cc Params.hh
```

## cmake support

```
# Function for creating TDRP Params.cc and Params.hh files

function(makeTdrpParams)

# Add a custom generator for TDRP Params.cc and Params.hh files
# from their associated paramdef.<app> file

set(TDRP_EXECUTABLE ${CMAKE_INSTALL_PREFIX}/bin/tdrp_gen)

add_custom_command (
OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/Params.hh ${CMAKE_CURRENT_SOURCE_DIR}/Params.cc
DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/paramdef.${PROJECT_NAME}
COMMAND cd ${CMAKE_CURRENT_SOURCE_DIR} && ${TDRP_EXECUTABLE}
-c++
-f paramdef.${PROJECT_NAME}
-prog ${PROJECT_NAME}
-add_ncar_copyright
COMMENT "Generating/updating Params.hh and Params.cc for ${PROJECT_NAME}"
)

endFunction()

# add tdrp_gen as a dependency

add_dependencies(${PROJECT_NAME} tdrp_gen)
```


