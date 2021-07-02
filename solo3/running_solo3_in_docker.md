# Running solo3 in a docker container

1. [prepare](#prepare)
2. [download](#download)
3. [run](#run)
--------------

<a name="prepare"/>

## 1. Prepare

### Install docker

See: [https://docs.docker.com/](https://docs.docker.com/)

### On a mac, install XQuartz

Since solo3 is an X application, you will need to install the XQuartz X server.

See:  [https://www.xquartz.org/](https://www.xquartz.org/)

After install, set the permissions to allow network clients:
 
```
  xQuartz -> Preferences -> Security -> Allow connections from network clients
  xQuartz -> Windows -> Click-through Inactive Windows
 ```

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

 * [run-solo3-in-docker.py](https://raw.githubusercontent.com/NCAR/lrose-displays/master/solo3/run-solo3-in-docker.py)

Right click on this link, and ```save as```.

It should save as:

```
  ~/Downloads/run-solo3-in-docker.py
```

Make the start script executable:

```
  chmod +x ~/Downloads/run-solo3-in-docker.py
```

The usage of the script is as follows:

```
Usage: run-solo3-in-docker.py [options]

Options:
  -h, --help            show this help message and exit
  --debug               Set debugging on
  --verbose             Set verbose debugging on
  --dir=DIR             Directory containing the DORADE data. Default is the
                        current directory.
  --docker_image=DOCKER_IMAGE
                        Set the docker image to run. Should be in DockerHub.
                        Default is "nsflrose/lrose-solo3"

```
--------------

<a name="run"/>

## 3. Run the script

You can either run the script from the dorade data directory, or specify the directory
when you call the script.

Suppose the dorade data is in ```/tmp/dorade```. Then the options would be:

```
  cd /tmp/dorade
  ~/Downloads/run-solo3-in-docker.py
```

or:

```
  ~/Downloads/run-solo3-in-docker.py --dir /tmp/dorade
```

