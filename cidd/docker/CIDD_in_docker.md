# Running CIDD in a docker container

## LINUX

You will need to install docker.

Then, run CIDD in a docker container, and point to a CIDD parameter file available on a web server somewhere.

For example, to run CIDD for RELAMPAGO:

```
docker run --net=host ncareol/lrose-cidd:latest /usr/local/cidd/bin/CIDD -font fixed -p http://front.eol.ucar.edu/displayParams/CIDD.relampago
```

## Apple Mac OSX

A good reference is:

https://cntnr.io/running-guis-with-docker-on-mac-os-x-a14df6a76efc

### Install XQuartz X server

Install XQuartz. Then configure it to accept network connections.

* start xterm
* XQuartz -> Preferences -> Security
* Select Allow connections from network clients

### Install docker

Download the docker.dmg, and install by clicking on the dmg file.

### Install the socat application

Install homebrew

Then:

```
  brew install socat
```

### Set up socat to connect X server port to container

If your shell is csh or tcsh, run the following command:

```
  socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\" >& /dev/null & 
```

If your shell is sh or bash, run the following command:

```
  socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\" > /dev/null 2>&1 & 
```

### Get your IP address

Run:

```
  ping `hostname`
```

Those quotations are back-quotes.

### Run CIDD in docker

Suppose the above ping indicates your IP address is 192.168.4.101.

Then run:

```
  docker run -e DISPLAY=192.168.4.101:0 ncareol/lrose-cidd:latest /usr/local/cidd/bin/CIDD -font fixed -p http://front.eol.ucar.edu/displayParams/CIDD.relampago
```





