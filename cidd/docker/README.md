# Running CIDD in a docker container

1. [prepare](#prepare)
2. [download](#download)
3. [run](#run)

<a name="prepare"/>

## 1. Prepare

### Install docker

See:

[https://docs.docker.com/](https://docs.docker.com/)

### On a mac, install XQuartz

Since CIDD is an X application, you will need to install the XQuartz X server.

See:

[https://www.xquartz.org/](https://www.xquartz.org/)

After install, set the permissions to allow network clients:
 
  xQuartz -> Preferences -> Security -> Allow connections from network clients

<a name="download"/>

## 2. Download the start script

The start script is checked into git:

[run-cidd-in-docker](https://raw.githubusercontent.com/NCAR/lrose-displays/master/cidd/scripts/run-cidd-in-docker)

Go there, and right click to ```save as```.

It should save as:

```
  run-cidd-in-docker.txt
```

Rename the file, and make it executable:

```
  mv run-cidd-in-docker.txt run-cidd-in-docker
  chmod +x run-cidd-in-docker
```

<a name="run"/>

## 3. Run the script

### Run using parameters in the cloud

A number of CIDD configuration files are on the NCAR/EOL/RSF web server:

[http://front.eol.ucar.edu/displayParams/](http://front.eol.ucar.edu/displayParams/)

To run with one of these files, you specify the URL.

The following are examples:

```
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.pecan
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.relampago
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.dynamo
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.eolbase
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.hawaii
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.ihop
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.improve
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.improveII
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.rico
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.cope
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.cp2_queensland
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.front
  run-cidd-in-docker http://front.eol.ucar.edu/displayParams/CIDD.name
```

### Run using a local parameter file

If you have a local CIDD parameter file, you can run with that file.

Suppose your parameters are called:

```
  CIDD.params
```

Then the command to use would be:

```
  run-cidd-in-docker CIDD.params
```

