# lrose-core software implementation

<img align="left" width="175" height="175" src="../images/LROSE_logo.small.png">
<img align="right" width="175" height="175" src="../images/spol_dynamo.jpg">

--------------------------------------------

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
* cmake
* gcc, g++ (or clang)
* libcurl, libpng, libtiff
* zlib, bzip2
* expat, flex
* fftw3
* X11
* qt5 or qt6
* hdf5, netcdf

Details on installing the packages for the build are found at:

* [LINUX](https://github.com/NCAR/lrose-core/tree/master/docs/build/lrose_package_dependencies.linux.md)
* [OSX](https://github.com/NCAR/lrose-core/tree/master/docs/build/lrose_package_dependencies.osx.md)

### INTERNAL DEPENDENCIES

`lrose-core` includes a series of libraries that are part of the build:

| Library | Description   |
| ------------- |:-------------:|
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

* [Libraries](https://github.com/NCAR/lrose-core/blob/master/docs/libs/lrose-libs-summary.pdf)

### BUILDS

For details on builds and the code development environment, see:

| Location      | Description   |
| ------------- |:-------------:|
| [Prepare for build](./docs/build/prepare_for_build.md) | Prepare you OS for building LROSE |
| [Build overview](./docs/build/LROSE_build_main_page.md) | Building and code development |

### REPOSITORY ORGANIZATION

| Location      | Description   |
| ------------- |:-------------:|
| [docs](./docs) | README files |
| [build](./build) | build scripts, release scripts and associated files |
| [codebase/libs](./codebase/libs) | library source code |
| [codebase/apps](./codebase/apps) | application source code |
| [lrose-bootstrap](https://github.com/NCAR/lrose-bootstrap) | Bootstrap scripts |

### DOCUMENTATION LINKS

| Name           | Description   |
| -------------- |:-------------:|
| [LROSE_DOCS_OVERVIEW.md](./docs/LROSE_DOCS_OVERVIEW.md) | Documentation index |
| [RadxDualPolApps.md](./docs/apps/radx/dualpol/RadxDualpolApps.md) | Dual-polarization apps |

### DOIs

The DOI for lrose-core is:

* [https://doi.org/10.5065/60hz-ry38](https://doi.org/10.5065/60hz-ry38)

The DOI entry information is stored at:

* [https://search.datacite.org/works/10.5065/60hz-ry38](https://search.datacite.org/works/10.5065/60hz-ry38)

### This page is published at:

[https://ncar.github.io/lrose-core](https://ncar.github.io/lrose-core)

