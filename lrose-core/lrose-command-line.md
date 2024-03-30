# `lrose-core` Command Line Conventions

## Single-dash options

The lrose apps generally use single dashes for the command line args.

## Standard arguments

The following are standard args on many applications:

```
  [-h, -help] produce this hdlp list
  [-d, -debug] print debug messages
  [-instance ?] specify instance for procmap
  [-v, -verbose] print verbose debug messages
  [-vv, -extra] print extra verbose debug messages
  [ -params path ] specify params file path
```

## Example - RadxMon

The following is an example of a full usage:

```
Usage: RadxMon [options as below]
options:
  [--, -h, -help, -man] produce this list.
  [-check_missing ?] check for missing beams
     Specify the max delta angle (deg) for normal ops.
  [-check_time_increasing ?] check for increaasing time
     Prints warning if time goes backwards.
  [-data] full plus data - very verbose
  [-d, -debug] print debug messages
  [-f, -files ???] specify input file list.
     Reads moments data from specified files.
     Sets FILE_INPUT mode.
  [-flags_summary] tilt flags in summary mode
  [-flags_monitor] flags in monitor mode
  [-fmq ?] input fmq url
     Sets FMQ_INPUT mode.
  [-full] scrolling, all params printed
  [-i, -instance ?] specify instance for procmap
  [-input_tcp_host ?] specify host for input tcp server
  [-input_tcp_port ?] specify port for input tcp server
     Sets TCP_INPUT mode.
  [-gate] print data for single gate
  [-georef] print platform georef info with each beam
     if available
  [-power] print power and frequency summary
  [-range] range for single gate print - km
  [-scan_name] add scan name at end of line in summary mode
  [-start] Seek to start of FMQ
     If not set, reading begins at the end of the FMQ.
  [-summary] scrolling summary
  [--summary_header_interval ?] header print interval (lines)
     in summary mode
  [-output_tcp_port ?] specify port for data output
     Port on which to listen for clients.
     Sets OPS_MODE_SERVER mode.
  [-subsecs_precision ?] set precision of subsecs in summary mode
     default is 0 - i.e. whole secs only
  [-update ?] update interval in seconds
     If 0, every beam printed.
     If < 0, updates on beam count instead of secs
  [-v, -verbose] print verbose debug messages
  [-vv, -extra] print extra verbose debug messages

TDRP args: [options as below]
   [ -params/--params path ] specify params file path
   [ -check_params/--check_params] check which params are not set
   [ -print_params/--print_params [mode]] print parameters
     using following modes, default mode is 'norm'
       short:   main comments only, no help or descr
                structs and arrays on a single line
       norm:    short + descriptions and help
       long:    norm  + arrays and structs expanded
       verbose: long  + private params included
       short_expand:   short with env vars expanded
       norm_expand:    norm with env vars expanded
       long_expand:    long with env vars expanded
       verbose_expand: verbose with env vars expanded
   [ -tdrp_debug] debugging prints for tdrp
   [ -tdrp_usage] print this usage
```

