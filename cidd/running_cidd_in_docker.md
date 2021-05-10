# Running CIDD in a docker container

1. [prepare](#prepare)
2. [download](#download)
3. [run](#run)
4. [docs](#docs)
--------------

<a name="prepare"/>

## 1. Prepare

### Install docker

See: [https://docs.docker.com/](https://docs.docker.com/)

### On a mac, install XQuartz

Since CIDD is an X application, you will need to install the XQuartz X server.

See:  [https://www.xquartz.org/](https://www.xquartz.org/)

After install, set the permissions to allow network clients:
 
```
  xQuartz -> Preferences -> Security -> Allow connections from network clients
  xQuartz -> Windows -> Click-through Inactive Windows
 ```

### Also on a mac, set defaults

Under Mac OS 10.4 and above, CIDD's popup panels do not function correctly by default. The Mac OS window manager does not give focus to the popup panels, so the user cannot select or change GUI elements.

To overcome this, start an xterm and run the following commands:

```
  defaults write com.apple.x11 wm_click_through -bool true
  defaults write com.apple.x11 wm_ffm -bool true
```

You can check your current defaults with:

```
  defaults read com.apple.x11
```

This only needs to be run once for each mac. You don't need to do this every time you run CIDD.

Then quit Xquartz and restart it. A reboot makes it clean.

After restarting XQuartz, open an xterm window, and in that window type:

```
  xhost +
```

or

```
  /opt/X11/bin/xhost +
```

--------------

<a name="download"/>

## 2. Download the start script

The start script is checked into git:

* [run-cidd-in-docker.py](https://raw.githubusercontent.com/NCAR/lrose-displays/master/cidd/scripts/run-cidd-in-docker.py)

Right click on this link, and ```save as```.

It should save as:

```
  ~/Downloads/run-cidd-in-docker.py
```

Make the start script executable:

```
  chmod +x ~/Downloads/run-cidd-in-docker.py
```

The usage of the script is as follows:

```
Usage: run-cidd-in-docker.py [options]

Options:
  -h, --help            show this help message and exit
  --debug               Set debugging on
  --verbose             Set verbose debugging on
  --docker_image=DOCKER_IMAGE
                        Set the docker image to run. Should be in DockerHub.
  --params=PARAMS       Set params file name. For example: 'CIDD.pecan'. In
                        this case the URL would be
                        'http://front.eol.ucar.edu/displayParams/CIDD.pecan'.
                        i.e. the param file name will be appended to the URL.
                        If the --params option is not used, then the
                        params_url will be used instead.
  --params_url=PARAMS_URL
                        Set the full URL for CIDD params file. This activates
                        if the --params option is not used.
  --params_local=PARAMS_LOCAL
                        Set path of local params file. This will be provided
                        to CIDD running in the container.
```
--------------

<a name="run"/>

## 3. Run the script

A number of CIDD configuration files are on the NCAR/EOL/RSF web server:

[http://front.eol.ucar.edu/displayParams/](http://front.eol.ucar.edu/displayParams/)

To run with one of these files, you can specify the full URL or just the parameter file.

### Run specifying just the parameter file name

To run with only the parameter file name:

```
  run-cidd-in-docker.py --params CIDD.rico
```

and the necessary URL will be supplied.

### Run specifying just the full parameter url

To run with the full parameter url:

```
  run-cidd-in-docker.py --params_url http://front.eol.ucar.edu/displayParams/CIDD.improve
```

### Run specifying a local parameter file

To run with a local file:

```
  run-cidd-in-docker.py --params_local ~/projDir/display/params/CIDD.pecan
```

This will only work if CIDD is set up to get the color scales, maps and data from a web server.

### List of available public parameter files

The following parameter files are available:

```
  CIDD.cp2_queensland
  CIDD.dynamo
  CIDD.eolbase
  CIDD.front
  CIDD.hawaii
  CIDD.ihop
  CIDD.improve
  CIDD.improveII
  CIDD.name
  CIDD.pecan
  CIDD.pre-cip
  CIDD.relampago
  CIDD.rico
  CIDD.timrex
```

### Saving images

If you save images or movies, they should appear in:

```
  /tmp/cidd_images
```

They will be owned by root, because inside the docker container, CIDD runs as root.

<a name="docs"/>

--------------

## 4. CIDD User Manual

See the [CIDD user manual](https://ncar.github.io/lrose-docs/cidd/user_manual/CIDD_manual.html)


