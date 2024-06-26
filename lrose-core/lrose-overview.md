# `lrose-core` software overview

## **LROSE** - The Lidar Radar Open Software Environment

LROSE is a co-operative project between:

  * [Dept. of Atmospheric Science at Colorado State University (CSU)](http://www.atmos.colostate.edu/) and the
  * [The Earth Observing Lab at the National Center for Atmospheric Research (NCAR)](https://www.eol.ucar.edu/content/lidar-radar-open-software-environment).

LROSE is funded by the [National Science Foundation](https://www.nsf.gov).

### LROSE CORE REPOSITORY

This GitHub distribution contains the core software for LROSE.

To download this repository from GitHub, run:

```
  git clone https://github.com/NCAR/lrose-core
```

### EXTERNAL DEPENDENCIES

`lrose-core` is dependent on the following major packages:

* git
* make, cmake
* python (releases, cmake support)
* gcc, g++ (or clang on OSX)
* libcurl (web-based data access)
* libpng, libtiff (image support)
* zlib, bzip2 (compression)
* expat (XML)
* flex (lexical analyzer)
* fftw3 (Fourier transforms)
* X11, qt5 or qt6
* hdf5, netcdf

As a rule, we have tried to keep the number of external dependencies to a minimum, and to only include packages that are universally available on the main LINUX versions, plus OSX under `brew`.

Details on installing the packages for the build are found at:

* [LINUX](https://github.com/NCAR/lrose-core/tree/master/docs/build/lrose_package_dependencies.linux.md)
* [OSX](https://github.com/NCAR/lrose-core/tree/master/docs/build/lrose_package_dependencies.osx.md)

### INTERNAL DEPENDENCIES

`lrose-core` includes a series of libraries that are part of the build:

| Library | Description   |
| ------------- |-------------|
| tdrp | Table-driven run-time parameters |
| dataport | Portable data types, byte swapping |
| toolsa | ANSI-C and C++ utilities |
| physics | Physics, meteorology |
| rapmath | Mathematics, statistics |
| FiltAlg, FiltAlgVirtVol | Spatial filtering |
| euclid | Euclidean geometry |
| advect | Meteorological advection routines |
| contour | Contouring routines |
| kd | KD-tree |
| hydro | Hydrology |
| rapformats | Binary representation of met. data types |
| didss | File access |
| dsserver, dsdata | Data server layer |
| grib, grib2 | GRIB data file decoding |
| shapelib | Shapefile data handling |
| radar | Radar-specific data and mathematics |
| Refract | Radar refractivity |
| Fmq | Fast Message Queue support |
| Mdv | Meteorological Data Volumes - gridded and Cartesian data |
| Ncxx | C++ NetCDF support |
| Radx | Handling radial data - radars and lidars |
| Spdb | Symbolic Product Data Base - time based storage of feature data |
| cidd, Solo, qtplot, rapplot | Plotting and display support |
| titan | Storm identification and tracking, spatial classification of radar data |

Further library details are available at:

* [Library descriptions](https://github.com/NCAR/lrose-core/blob/master/docs/libs/lrose-libs-summary.pdf)

### LROSE repositories

The LROSE code, scripts, parameter files and documentation is checked into GitHub under the NCAR organization.

The repository details are as follows:

| Name | Repository | Description   |
| ---- | ------------- |:-------------:|
| lrose-core | [github.com/NCAR/lrose-core](https://github.com/NCAR/lrose-core) | Core C and C++ code |
| lrose-bootstrap | [github.com/NCAR/lrose-bootstrap](https://github.com/NCAR/lrose-bootstrap) | Checkout, build, release |
| lrose-docs | [github.com/NCAR/lrose-docs](https://github.com/NCAR/lrose-docs) | Overall documentation |
| lrose-devel | [github.com/NCAR/lrose-devel](https://github.com/NCAR/lrose-devel) | Development and test environment |
| lrose-test | [github.com/NCAR/lrose-test](https://github.com/NCAR/lrose-test) | Development and test environment |
| lrose-example | [github.com/NCAR/lrose-example-data](https://github.com/NCAR/lrose-example-data) | Data for testing |
| lrose-titan | [github.com/NCAR/lrose-titan](https://github.com/NCAR/lrose-titan) | TITAN project scripts and parameters |
| lrose-projects-eolbase | [github.com/NCAR/lrose-projects-eolbase](https://github.com/NCAR/lrose-projects-eolbase) | EOL projects scripts and parameters |
| lrose-cedric | [github.com/NCAR/lrose-cedric](https://github.com/NCAR/lrose-cedric) | Code for legacy cedric app |
| lrose-soloii | [github.com/NCAR/lrose-soloii](https://github.com/NCAR/lrose-soloii) | Code for legacy soloii app |
| lrose-solo3 | [github.com/NCAR/lrose-solo3](https://github.com/NCAR/lrose-solo3) | Code for legacy solo3 app |
| CfRadial | [github.com/NCAR/CfRadial](https://github.com/NCAR/CfRadial) | Docs for CfRadial NetCDF format |
| fractl | [github.com/mmbell/fractl](https://github.com/mmbell/fractl) | FRACTL wind retrieval application (CSU) |
| samurai | [github.com/mmbell/samurai](https://github.com/mmbell/samurai) | SAMURAI wind retrieval application (CSU) |
| vortrac | [github.com/mmbell/vortrac](https://github.com/mmbell/vortrac) | VORTRAC hurricane analysis application (CSU) |

### DOCUMENTATION LINKS

| Name           | Description   |
| -------------- |:-------------:|
| [LROSE_DOCS_OVERVIEW](https://github.com/NCAR/lrose-core/blob/master/docs/LROSE_DOCS_OVERVIEW.md) | Documentation index |
| [RadxDualPolApps](https://github.com/NCAR/lrose-core/blob/master/docs/apps/radx/dualpol/RadxDualpolApps.md) | Dual-polarization apps |
| [TDRP](http://htmlpreview.github.io/?https://github.com/NCAR/lrose-core/blob/master/docs/tdrp/index.html) | TDRP implementation details |

### DOIs

The DOI for lrose-core is:

* [https://doi.org/10.5065/60hz-ry38](https://doi.org/10.5065/60hz-ry38)

The DOI entry information is stored at:

* [https://search.datacite.org/works/10.5065/60hz-ry38](https://search.datacite.org/works/10.5065/60hz-ry38)

