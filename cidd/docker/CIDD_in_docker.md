# Running CIDD in a docker container

## LINUX

You will need to install docker.

Then, run CIDD in a docker container, and point to a CIDD parameter file available on a web server somewhere.

For example, to run CIDD for RELAMPAGO:

```
docker run --net=host ncareol/lrose-cidd:latest /usr/local/cidd/bin/CIDD -font fixed -p http://front.eol.ucar.edu/displayParams/CIDD.relampago
```

## Apple Mac OSX

### Install docker

Download the docker.dmg, and install by clicking on the dmg file.

## Install the socat application

Install homebrew

Then:

  brew install socat


