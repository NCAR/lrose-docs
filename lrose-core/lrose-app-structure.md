# `lrose-core` Anatomy of a typical LROSE application

## Common source files

Many LROSE apps have a similar code structure at the top level.
(Here we use the Radx2Grid application as an example.)

| Source file | Description | Example    |
| -------------     |:-------------:|:----------|
| Main.cc  | Main entry point | [Main.cc](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/Radx/src/Radx2Grid/Main.cc) |
| Radx2Grid.hh  | Top-level class | [Radx2Grid.hh](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/Radx/src/Radx2Grid/Radx2Grid.hh) |
| Radx2Grid.cc  | Top-level class | [Radx2Grid.cc](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/Radx/src/Radx2Grid/Radx2Grid.cc) |
| Args.hh  | Command line args | [Args.hh](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/Radx/src/Radx2Grid/Args.hh) |
| Args.cc  | Command line args | [Args.cc](https://github.com/NCAR/lrose-core/tree/master/codebase/apps/Radx/src/Radx2Grid/Args.cc) |

## Main.cc

This is the main entry point, and contains:

```
  int main(int argc, char **argv);
```

We set the signal handling to respond to termination, and to avoid unwanted interrupts from SIGPIPE signals.

We launch the main class.

And we set up a function to clean up on exit.

## AppName.hh

The main application class generally has the following members at a minumum:

```
  string _progName;  // app name
  char *_paramsPath; // path to parameter file
  Args _args;        // command line parsing
  Params _params;    // TDRP parameters
```

## AppName.cc

The constructor for the main application class generally contains the following code, as a mimumum:

```
  // set programe name

  _progName = "Radx2Grid";
  ucopyright((char *) _progName.c_str());
  
  // parse command line args
  
  if (_args.parse(argc, argv, _progName)) {
    cerr << "ERROR: " << _progName << endl;
    cerr << "Problem with command line args." << endl;
    OK = FALSE;
    return;
  }
  
  // get TDRP params
  
  _paramsPath = (char *) "unknown";
  if (_params.loadFromArgs(argc, argv,
			   _args.override.list, &_paramsPath)) {
    cerr << "ERROR: " << _progName << endl;
    cerr << "Problem with TDRP parameters." << endl;
    OK = FALSE;
    return;
  }

  // initialize registration with procmap
  
  PMU_auto_init(_progName.c_str(),
                _params.instance,
                PROCMAP_REGISTER_INTERVAL);

  // initial regsitration

  PMU_auto_register("Init");
```

## Args.hh

Class to parse the command line.

Args generally have a list of overrides, to override TDRP parameters:

```
  tdrp_override_t override;
```

The override list is defined in ```libs/tdrp/src/include/tdrp/tdrp.h```:

```
  typedef struct {
    char **list;
    int n;
  } tdrp_override_t;
```

See Args.cc below for how this is used.

## Args.cc

In Args.cc we process the command line arguments and respond appropriately.

The convention is that the command line overrides the parameters in the params file.

So in the command line loop, you will see examples similar to the following:

```
  for (int i =  1; i < argc; i++) {
    
    if (!strcmp(argv[i], "--") ||
	!strcmp(argv[i], "-h") ||
	!strcmp(argv[i], "-help") ||
	!strcmp(argv[i], "-man")) {
      
      _usage(cout);
      exit (0);
      
    } else if (!strcmp(argv[i], "-d") ||
               !strcmp(argv[i], "-debug")) {
      
      sprintf(tmp_str, "debug = DEBUG_NORM;");
      TDRP_add_override(&override, tmp_str);
      
    } else if (!strcmp(argv[i], "-v") ||
               !strcmp(argv[i], "-verbose")) {
      
      sprintf(tmp_str, "debug = DEBUG_VERBOSE;");
      TDRP_add_override(&override, tmp_str);

    } ...
```

Here we construct strings and add them to the override list.
The overrides are appended to the parameter file buffer before parsing.
Parameters that appear later override those that appear earlier.

You can see the parameters to be parsed by adding the arg ```-tdrp_debug``` to the command line.
That will force a listing of the parameters to ```stderr```.


