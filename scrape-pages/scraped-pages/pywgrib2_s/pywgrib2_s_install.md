### Installation

### Requirements:

1. python 3.5+, numpy

- Linux (tested: Ubuntu, Redhat, SUSE), MacOS
- wgrib2 v3.0.2+ shared library

### Installation:

Step 1: Make or obtain a wgrib2 shared library2. To make the wgrib2 shared library

- cd to the directory where you want to place the wgrib2 source code: cd XYZ
- get the latest source code to wgrib2 (wgrib2.tgz):
- $ wget "https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/wgrib2.tgz"
- untar the source code: $ tar -xzvf wgrib2.tgz
- cd to the grib2 directory: $ cd grib2
- edit makefile, change "MAKE_SHARED_LIB=0" to "MAKE_SHARED_LIB=1"
- optional edit makefile to user customizations
- set the C and fortran compilers (must be gcc and gfortran), this is the bash compile
- $ export CC=gcc
- $ export FC=gfortran
- $ make lib
- check for the shared library, linux: lib/libwgrib2.so, mac: lib/libwgrib2.dylib
  Step 2: find a directory on the python path (PYDIR)2. try .local/lib/pythonN.M/site-packages/ N.M = python version N.M >= 3.5 (lowest version tested)

```
   # .local/lib/pythonN.M/site-packages will not appear on search path unless already created
   cd
   mkdir -p .local/lib/pythonN.M/site-packages
```

- query python for directories

```
   >>> import sys
   >>> sys.path
```

- The above script will not show missing directories on the path.
- Choose an appropriate directory with write permission.
- If you cannot find a writable directory, change the python path to create such a directory.
  Step 3: Get pywgrib2_s.py PYDIR2. download https://ftp.cpc.ncep.noaa.gov/wd51we/pywgrib2\_s/pywgrib2\_s.tgz
- extract contents, tar -xzvf pywgrib2_s.tgz
  Step 4: install wgrib2 shared library 2. shared library is named libwgrib2.so (use libwgrib2.dylib for Darwin)
- for pywgrib2_s version 0.0.x, copy shared wgrib2 lib to PYDIR
- pywgrib2_s 1.0.0+ has a search order for the shared library. 1. name of shared library is given by environment varible $WGRIB2_LIB - shared library (libwgrib2.so) is saved in PYDIR - shared library (libwgrib2.so) is found in system defined library
  Step 5: test using the cookbook2. wget https://ftp.cpc.ncep.noaa.gov/wd51we/pywgrib2\_s/pywgrib2\_s\_cookbook.tgz
- Try running the cookbook\_\*.py scripts from the shell.

### Multiple versions of shared wgrib2 library:

The libraries used by pywgrib2_s may conflict with those used by
the calling program. In such cases, you have to compile wgrib2 without
the offending libraries. This may reduce the functionality of pywgrib2_s/wgrib2
but you will have to work around this problem. The end result can be
multiple versions of libwgrib2.so. In such cases,
the environment variable $WGRIB2_LIB will have to be used to specify
except for the default library.

### System Installations

The above instructions suggested that .local/lib/pythonN.M/site-packages
may work for PYDIR. This suggestion is for an person to install on one's account. For
a system install, you need to be root and place it in a root owned
directory on the python path.

See also:

---

> Description: Install pygrib2_s_install

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pywgrib2_s_install.html>_
