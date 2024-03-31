# `lrose-core` Anatomy of a typical LROSE application

## Common source files

Many LROSE apps have a similar code structure at the top level.
Here we use the Radx2Grid application as an example.

| Source file | Description | Example    |
| -------------     |:-------------:|:----------|
| Main.cc  | Main entry point | [Main.cc](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/Radx/src/Radx2Grid/Main.cc) |
| Radx2Grid.hh  | Top-level class | [Radx2Grid.hh](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/Radx/src/Radx2Grid/Radx2Grid.hh) |
| Radx2Grid.cc  | Top-level class | [Radx2Grid.cc](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/Radx/src/Radx2Grid/Radx2Grid.cc) |
| Args.hh  | Command line args | [Args.hh](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/Radx/src/Radx2Grid/Args.hh) |
| Args.cc  | Command line args | [Args.cc](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/Radx/src/Radx2Grid/Args.cc) |

* M

Most LROSE apps use TDRP (Table Driven Runtime Parameters) to specify the parameters for an app.

TDRP provides typed parameter definitions, plus documentation, that becomes compiled into the application.

See the full TDRP docs at:

  http://htmlpreview.github.io/?https://github.com/NCAR/lrose-core/blob/master/docs/tdrp/index.html

## **paramdef** file

The paramters are defined in a ```paramdef``` file.

The following paramter types are supported:

* string (or char *)
* boolean
* int
* long
* float
* double
* enum
* struct
* arrays of any of the above types

In addition, for each parameter, the following may be specified:

* descr - description
* help
* min, max valid values for numerics
* default value

The paramdef file for the drpTest application is here:

* [paramdef.TdrpTest](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/tdrp/src/TdrpTest/paramdef.TdrpTest)

## Code generation

The ```tdrp_gen``` app reads the paramdef file, and generates a class, generally called ```Params```.

The command line for tdrp_gen is as follows:

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

## Code location

| TDRP code         | URL      |
| -------------     |:-------------:|
| lib       | https://github.com/NCAR/lrose-core/tree/master/codebase/libs/tdrp |
| apps      | https://github.com/NCAR/lrose-core/tree/master/codebase/apps/tdrp |

The most important TDRP app is `tdrp_gen`.

## Makefile

In a standard Makefile, we handle tdrp using the following rules:

```
Params.hh: Params.cc

Params.cc: paramdef.appName
        tdrp_gen -f paramdef.appName -c++ -prog appName -add_ncar_copyright

clean_tdrp:
        /bin/rm Params.cc Params.hh
```

## cmake

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


