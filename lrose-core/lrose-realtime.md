# Real-time operations

## Overview

There are a number of components that make up the LROSE realtime system.

| Component | Description |
| -----     | ----------- |
| procmap   | Process mapper. This is a server. Individual applications register routinely with procmap to confirm that they are alive and running as expected. |
| apps      | Applications register with procmap |
| proc_list | List of required applications |
| auto_restart | Auto restarter - reads proc_list, checks with procmap if all are running correctly. If a process is not registering correctly, it will kill it (in case it is hung) and restart it. (perl) |
| procmap_list_start | Start script - reads proc_list, starts apps. (perl) |
| procmap_list_kill | Kill script - reads proc_list, kills apps. (perl) |
| cron | Keeps auto_restart running. Performs other tasks on a schedule as required |
| DataMapper | Data mapper. This is a server. Applications register with DataMapper when they write data. |
| start_all | Overall start script. (shell, generally c-shell) |
| stop_all | Overall stop script. (shell, generally c-shell) |
| print_procmap | App to print the procmap status. |
| PrintDataMap | App to print the DataMapper status. |
| LogFilter | This is an application that reads stdout and stderr from apps, logs the text to date-stamped files in a designated directory. |

The figure below shows how these components interact:

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

## ```proc_list``` - the process list

The process list controls which processes (i.e. instances of applications) should be run in the system. It specifies the process name, the instance and the start and kill scripts for the process. The hostname is included for backward compatibility and should always be set to localhost.

The following is an example of a proc_list file, taken from the lrose project templates template_single_radar:

```
########################################################################
# Example proc_list file
#
# name       instance   start_script          kill_script       hostname
########################################################################
# SYSTEM processes
#
DsServerMgr   primary    start_DsServerMgr    snuff_inst        localhost
Janitor       primary    start_Janitor        kill_Janitor      localhost
Scout         primary    start_Scout          kill_Scout        localhost
DataMapper    primary    start_DataMapper     kill_DataMapper   localhost
#########################################################################
# INGEST processes
#
Bprp2Dsr       ops       start_Bprp2Dsr.ops        snuff_inst  localhost
EsdAcIngest    ops       start_inst(ingest)        snuff_inst  localhost
Dsr2Vol        ops       start_Dsr2Vol.ops         snuff_inst  localhost
ClutterRemove  cart      start_inst(ingest)        snuff_inst  localhost
########################################################################
# lrose ROCESSES
#
lrose          ops       start_lrose.ops           snuff_inst  localhost
PrecipAccum    single    start_PrecipAccum.single  snuff_inst  localhost
PrecipAccum    1hr       start_PrecipAccum.1hr     snuff_inst  localhost
PrecipAccum    24hr      start_PrecipAccum.24hr    snuff_inst  localhost
Mdv2Vil        ops       start_Mdv2Vil.ops         snuff_inst  localhost
Tstorms2Spdb   ops       start_Tstorms2Spdb.ops    snuff_inst  localhost
########################################################################
# DISPLAY processes
#
Rview          ops       start_Rview.ops      snuff_inst       localhost
TimeHist       ops       start_Rview.ops      snuff_inst       localhost
RadMon         ops       start_RadMon.ops     kill_RadMon.ops  localhost
CIDD           ops       start_CIDD.ops       snuff_inst       localhost
```

The process binary must be in the search path. The instance for a process is used to distinguish between different instances of the same process. In the example above, PrecipAccum is running with 3 different instances, one to convert single radar scans into precipitation amounts and the other two to accumulate precipitation into 1 and 24 hour running totals.

If a specific start script for a process exists, it should be specified. If not, the ```start_inst``` mechanism may be used. More details on this later on.

If special action must be taken to kill the application, a kill script should also be supplied. However, if nothing special is needed to kill the application the entry ```snuff_inst``` can be used instead. Based on that entry the system will kill the application based on its name and instance.

This script is called at system shutdown to go through the proc_list and kill all processes by calling the kill script or the ```snuff_inst``` mechanism.

## cron table file

The cron daemon on a UNIX system is designed to run tasks on a predefined schedule. The so-called cron table is used to specify which tasks are to be scheduled.

Below is a typical crontab file for a lrose real-time system:

```
##########################################################################
#
# Example cron table for lrose
#
# Process restarters
*/1 * * * *  csh -c “start_auto_restart_check_cron” 1> /dev/null 2> /dev/null
*/1 * * * *  csh -c “start_procmap_check_cron”      1> /dev/null 2> /dev/null
#
# Build links to log date subdirs
*/5 * * * *   csh -c “start_build_logdir_links” 1> /dev/null 2> /dev/null
#
```

There are 3 scheduled tasks:
* every 1 minute the script start_auto_restart_check_cron is run to ensure that the auto_restart script is running.
* every 1 minute the script start_procmap_check_cron is run to ensure that procmap is running.
* every 5 minutes start_build_logdir_links runs to create symbolic links in the log directories to point to log files for yesterday and today. The log files are stored in directories named for the date, i.e. yyyymmdd. The links are a convenient way to easily find the log files for today and yesterday.

## Log files.

There are 3 possible sets of log files:

| Purpose | Description | Normal location |
| -----   | ----------- | --------------- |
| errors  | Error and warning messages from processes. | ```~/projDir/logs/errors``` |
| restart | Restart messages from auto_restart. | ```~/projDir/logs/restart``` |
| distrib | Messages from file distribution, using DsFileDist app. | ```~/projDir/logs/distrib``` |

The logs files themselves reside in subdirectories named using the date. As an example, the error logs for 2023/09/25 will be written to ~/projDir/logs/errors/20230925.

The error logs contain all error messages generated by the processes themselves. The log files are named after the process name and instance. For example, the PrecipAccum application running as instance 24hr will create a log file called PrecipAccum.24hr.log.

The restart logs are written by the auto_restart script and show all restart activity.

We generally set up a number of environment variables to control the locations of the log files. The following is an example, using c-shell syntax:

```
setenv LOG_DIR $DATA_DIR/logs
setenv ERRORS_LOG_DIR $LOG_DIR/errors
setenv RESTART_LOG_DIR $LOG_DIR/restart
setenv DATA_DISTRIB_LOG_DIR $LOG_DIR/distrib
```

For convenience links are provided in $LOG_DIR to the logs from yesterday and today. The link is updated every 5 minutes (see cron) using the script ```start_build_logdir_links```.

The links would be something like the following:

```
  today -> /home/rsfdata/data/logs/errors/20240331
  yesterday -> /home/rsfdata/data/logs/errors/20240330
```

## LogFilter

The log files are managed by piping the stderr and stdout output from processes through a specially-written filter called LogFilter. This application reads data from standard input and writes it to a daily file.

```LogFilter``` is a C++ application that is designed to read in the output streams (stdout and stderr) from upstream processes, duplicate this stream, and write it to log files in a designated location. See the 'Log files' section above.


The following is an example of a start script that invoked the LogFilter:

```
#! /bin/csh
# use MDV because it is more efficient not to go through netcdf
setenv MDV_WRITE_FORMAT FORMAT_MDV
setenv MDV_WRITE_USING_EXTENDED_PATHS TRUE
cd $PROJ_DIR/ingest/params
running "MdvMerge2 -params MdvMerge2.3D_mosaic"
if ($status == 1) then
    MdvMerge2 -params MdvMerge2.3D_mosaic -debug |& \
	LogFilter -d $ERRORS_LOG_DIR -p MdvMerge2 -i 3D_mosaic >& /dev/null &
endif
```
