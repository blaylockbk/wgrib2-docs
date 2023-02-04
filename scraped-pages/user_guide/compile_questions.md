### Compiling wgrib2 v3.0.2+

```
1) Download ftp://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/wgrib2.tgz
        or https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/wgrib2.tgz
2) remove pre-existing grib2 directory if exists: rm -r grib2
3) untar wgrib2.tgz:  tar -xzvf wgrib2.tgz   (use gnu tar)
4) cd to main directory:  cd grib2


The makefile uses two and one optional environment variables that have to be set.
  With wgrib2 v3.0.2, you need to set $CC and $FC, and the makefile tries to identify
    $COMP_SYS from "uname -a".  However, if the makefile cannot identify your system,
    $COMP_SYS will not be set.  If your compilers are gcc and gfortran, you can
    try compiling wgrib2 with COMP_SYS=gnu_linux.


  linux, bsd-type OS, gcc/gfortran compilers:  COMP_SYS=gnu_linux
                                               CC=gcc
                                               FC=gfortran
  linux, AOCC:                                 COMP_SYS=clang_linux
                                               CC=clang
                                               FC=flang
  linux, icc and ifort:                        COMP_SYS=intel_linux
                                               CC=icc
                                               FC=ifort
  linux, icx and ifx:                          COMP_SYS=oneapi_linux
                                               CC=icx
                                               FC=ifx
  linux, Nvidia HPC SDK                        COMP_SYS=nvidia_linux
                                               CC=nvc
                                               FC=nvfortran

  Windows, cygwin gcc and gfortran:            COMP_SYS=cygwin_win
                                               CC=gcc
                                               FC=gfortran

  MacOS, real gcc and gfortran:                COMP_SYS=gnu_mac
                                               CC=gcc
                                               FC=gfortran



  not recently tested AIX, gnu_linux_g95, open64
  If COMP_SYS is not defined, the makefile will attempt to determine the COMP_SYS.

6) set environment variables and make, example

   export CC=gcc                    (bash version)
   export FC=gfortran
   export COMP_SYS=gnu_linux        (optional in linux)
   make                             (to make wgrib2)
   make lib                         (to make wgrib2 library)

   For NCEP's WCOSS-2.  see the question on lib64, and WCOSS-2.

```

### Intel Compilers

The Intel C compiler will not compile the Jasper library (jpeg2000 support).
To get jpeg2000 support, you will either have to use OpenJPEG or compile
Jasper with the gnu compiler. The makefile will automatically use gcc when
trying to build the Jasper library when the classic Intel compilers are used.
Compiling wgrib2 using the Intel compilers on Windows is possible but is
not supported. (Grib files will be limited to 2 GB, and I have no way to test.)

To compile with the intel compilers on linux,

```

Classic compilers
   export CC=icc
   export FC=ifort
   export COMP_SYS=intel_linux
   make

LLVM compilers (wgrib2 v3.1.2+)
   export CC=icx
   export FC=ifx
   export COMP_SYS=oneapi_linux
   make

```

### Compiling with Cygwin (Windows)

The only Windows C compiler supported is Cygwin's gcc.
The other Windows C compilers follow Microsoft's lead where
a "long int" is 32-bits on a 64 bit operating system. Wgrib2
will work with a 32-bit long int but will limit a grib file
to barely acceptable 2GB size. Cygwin's gcc also supports POSIX
which means that you don't have to turn off features that require POSIX.

### Compiling in MacOS

You need to use compile with gcc and gfortran. The default MacOS
installation has gcc pointing to clang. You can get the real deal
from homebrew.

With wgrib2 v3.0.0, MacOS support is now builtin. For prior
releases, there have been nice pages which detail the compiling process.

### Compiling with other compilers

There is no support for other compilers. At one time, AIX was
supported until our machines were scraped (2012). Use to support
gcc/g95 and the open64 compilers until development was stopped on
g95 and open64. I haven't tried the Cray compilers because
the Crays are already well supported by gcc/gfortran, icc/ifort
and clang/flang.

While there is no support for other compilers, the wgrib2 source
code was written to the OS, compiler and hardware independant. The only
limitation is that integers need to be 32+ bits long. Only compiler/hardware
feature is an optional optimization to replace a loop with a \_\_builtin_clz()
which works when GNUC >= 4. The main difficulties with porting to another
system should be in the libraries and disabling POSIX features.

### Makefile Options

There are many options that are documented in the makefile (grib2/makefile).
There are options to remove features that require POSIX support. There are
options to remove libraries that are not public domain or not under a GNU license.
There are options for code that may be difficult to cross-compile.

### Python Support, make a shared library

Python support requires a shared wgrib2 library. This option
only works for gnu/linux, gnu/MacOS, gnu/Windows, nvidia/linux AOCC/linux, icx/ifx.

```

  MAKE_SHARED_LIB=1
   note: to make the shared library $ make clean
                                    $ make lib
                                    shared library will be in lib/

```

### Grib names

With wgrib2 v3.0.2, the grib variable names are shown using the
DWD, ECMWF or NCEP tables. The locally defined variable names are
shown by the table of the local center. This option sets the default
grib table.

Not all names could be
accommadated because some of the names had level, timing information
This feature is more beta because of the difficulty with level
and timing information in the variable names.

```

  USE_NAMES=NCEP          use NCEP name (default)
  USE_NAMES=ECMWF         use ECMWF names
  USE_NAMES=DWD1          use DWD names, DWD has two center ids.

```

### NetCDF

- Option 1: No NetCDF support
  - fastest compile
  - small executable (3.9 MB as of 2014)
  - compile works on 32+ bit machines
  - cannot read nor write NetCDF files
  - no library conflicts in wgrib2lib when calling program
    uses netcdf or hdf
  - makefile configuration: USE_NETCDF3=0, USE_NETCDF4=0\* Option 2: NetCDF3 support (default in makefile)
  - fast compile
  - modest increase in executable size (5.7 MB vs 3.9 MB as of 2014)
  - compile may fail on 32-bit machines
  - library conflict in wgrib2lib when calling program that uses netcdf
  - makefile configuration: USE_NETCDF3=1, USE_NETCDF4=0\* Option 3: NetCDF4 compile libraries
  - slow compile time (hours on an Intel Apollo Lake)
  - 3.5x increase in executable size (13.5 MB vs 3.9 MB as of 2014)
  - makefile configuration: USE_NETCDF3=0, USE_NETCDF4=1
  - HDF5-1.10.4: compile works on RedHat 6 (gcc 4.x and intel 17.03)
  - HDF5-1.10.4: compile has failed on Ubuntu and various other systems (newer gcc)
  - HDF5-1.10.4: compile has failed on Ubuntu and AOCC (clang variant)
  - HDF5-1.10.6: compiles on Ubuntu and many other machines
  - HDF5-1.10.6: requires fortran90 compiler
  - HDF5-1.10.6: wgrib2 fails with hdf5 internal error
  - library conflict in wgrib2lib when calling program that use netcdf
  - makefile configuration: USE_NETCDF3=0, USE_NETCDF4=1
  - prompts for downloading netcdf4/hdf5 libraries\* Option 4: NetCDF4 external libraries (v3.1.2+)
  - User is resonsible for validating the results using the netcdf4/hdf5 libraries.

The netCDF4 option is currently unsupported (10/2020). The previous
release of hdf5 would not compile with modern gcc compilers. According
to the release notes, the current release of hdf5 supports more
modern version of gcc but not the latest versions. In personal testing,
I had to revert to an older version of hdf5 to make wgrib2 work. It
may be a problem with my code. However, the older version will not
compile with even a moderately modern gnu compiler. Older versions
of ihe Intel compiler were fine, but the latest version had problems (1/2021).
Preliminary testing of wgrib2 v3.1.1 was promising for netcdf4.

Probably every OS includes precompiled netcdf4 and hdf5 libraries. Modifying the
wgrib2 makefile to use the system netcdf4 files will depend on the system and
the source of the netcdf4 libraries.

### JPEG2000: Jasper, OpenJPEG

With wgrib2 v3.0.0, jpeg2000 compression can be handled by either the
Jasper or OpenJPEG library. Both libraries are equally slow and files are
roughly the same size. The advantages with
the Jasper library is that the last problem with the Jasper library was fixed
may years ago. The disadvantage with Jasper is that support may be lacking and
it doesn't compile with several C compilers.
The advantage with OpenJPEG is the support is ongoing. The disadvantage is that it requires
cmake to build. Consequently I cannot build and test the OpenJPEG version on my linux
workstation at work. Eventually OpenJPEG will be the default option.

### Some libraries are being created in grib2/lib64

Normally the libraries created by the wgrib2 build are stored in
grib2/lib. However, I found one machine that put some libraries in grib2/lib64.
One builds many of the libraries by first configuring the build by running
the "./configure" program. The "./configure -help" command lists the various
values.
However, if $CONFIG_SITE is defined, the $CONFIG_SITE script can change
can alter the default values or even more. In my case, you can
work around this $CONFIG_SITE by

```

$ make
  (crash)
$ cp -r lib64/* lib/
$ make
  (builds wgrib2/wgrib2)

```

### Missing zlib.h

I haven't been able to reproduce this compiling problem. The png compression requires
both the zlib library and the zlib.h include. I suspect that the libpng configure program
is using the system zlib because it finds the system zlib library. Perhaps the fix is
to install the zlib.h files by installing the "zlib-develop" package. Another fix
is to remove the system zlib and let libpng use the wgrib2-supplied zlib. The third
fix is to diable png compression in the make file. PNG compression
is not very good and I have only seen it used by RADAR files from ESRL.

### wgrib2 compile questions

```
Question: Warnings about parameter type mismatch in fftpack.f"?

Answer:
   The file fftpack.f is was written to an old fortran standard
before you had an allocatable array and memory was at a premium.
To save space, a real work array was defined and used in some parts
of the code.  The same array was used in other parts of the code
which needed an integer work array.  You save memory by using the
same array in both places.  It was common practice and the old
compilers didn't complain.  Ignore the warnings.

```

```
Question: Why do you not support "make install"?

Answer:

  "Make install" is not supported by the makefile because
installation is just copying the executable to another location.
There is no obvious default for all systems.

```

```
Question: Why do you build japser with the following flags?

     --disable-libjpeg --disable-opengl

Answer:

Libjpeg is not needed by wgrib2.  The makefile should work when
the system doesn't have libjpeg installed.

Opengl is not needed by wgrib2. Makefile should still work when the
system doesn't have opengl installed.

The wgrib2 makefile compiles the libraries in a manner optimized for wgrib2.
The libraries are not meant for general use.  For example, the netcdf library
doesn't have a fortran interface because I want the makefile to
work if a user lacks a fortran compiler.

```

```

Question: Why do build zlib and libpng?

Answer:  Not all linux distributions include libpng or they give it a
different name.  Some linux distributions may have different versions
of libpng and zlib.

```

```
Question: Why don't you make netcdf4 the default netcdf package?

Answer:

The Netcdf4/hdf5 libraries are very big and the end result is a long
download time and a large executable. If you decide to compile with
Netcdf4, the make will fail with the commands needed to download
the netcdf4 and hdf5 libraries.  After you have download the libraries,
the make should work.

At the current time (9/2020), compiling with the netcdf4 option has been
unsupported for a few years.  (Classes of users will have problems and nothing
can be done.) Until recently, the hdf5 library had problems being compiled
with the newer versions of gcc.  The latest version will compile with the
gcc included with Ubuntu.  However the newest version breaks the -import_netcdf
option with a uninformative hdf5 error message.

```

```
Question: Why don't you automatically download the netcdf and hdf5 packages?
Answer:

   People concerned about security would not like that action.

```

```
Question: I get the error messagee: /bin/sh: ./configure: Permission denied

Answer:

It is possible to configure a filesystem so that the execute bit is
set to zero. Programs and scripts will not execute when they reside
on that filesystem.  Try compiling on another filesystem.  If that doesn't
work, you are probably not allowed to download programs.  The solution
is to ask that your sys admin to compile the program for you.  But if
your sys admin will compile code for you, why are you reading this page?
Let him read this page.

```

```
Question: Why the options to turn off aec, jpeg2000 and png compression

Answer:

Some HPC machines use one type of cpu and/or OS to compile and
another type of cpu and/or OS to run (cross-compile).  Using configure
scripts can be a challenge when cross-compiling.  I wanted a reasonable
wgrib2-subset that can be cross-compiled.  Consequently all libraries that
that require a configure script are optional.

The configure programs of various libraries may not handle certain
machines or compilers.  For example an ARM user had to turn off the
jasper library.  Clang on the MacOS may be a problem for the AEC
library.

```

```
Question: Why does compiling wgrib2 with the Netcdf option fail?

Answer:

This frequently happens with 32-bit machines.  This problem goes away
on a 64-bit platform or if netcdf-4 is used instead of netcdf-3. This
problem is not being addressed because of a lack of 32-bit machines.

Some versions of hdf5 will not compile with modern C compilers.

```

```
Question: Why the option to turn off g2clib?

Answer: g2clib is not needed but it can be installed for testing at NCEP.

```

```
Question: Why gctpc and proj4? Why not proj4 without gctpc?

Answer: Gctpc and proj4 are both projection libraries and only one is
really needed.  Proj4 has more functionality, is modern and has an
active support group.  Gctpc is old and is more-or-less unsupported.
On the other hand, gctpc is simpler, works and supports OpenMP like the
rest of wgrib2.  Proj4 supports a different threading model.  As
grids get larger, you want the speed of gctpc. Both libraries are being
supported in order to help debugging and in the case that a future
grid requires Proj4 support.

```

```
Question: Why so many compile options?

Answer:
  Some systems or compilers do not support certain POSIX features
  Some functionality is not needed by most systems.
  Not all systems have fortran, MySQL, or support NetCDF/HDF5.
  The code that calls IPOLATES is not using a standard interface.
  Some systems do not handle cross-compiling easily.
  Some uses of wgrib2 may require a small RAM usage.
  Same makefile is used to compile the wgrib2 library.
  Some compilers may not support C99 or C11 (future).

```

```
 Problem: I compiled wgrib2 and when I run it, it complains about
      missing libraries.

Answer:
  I have the same problem with our Cray.  If you compile with the Intel
  compilers, you have to load the Intel environment before running the
  executables.  If you compile with the GNU compilers, you have to load
  the GNU environment before running.  The way to get around this
  problem is to compile wgrib2 with the same compiler that was used
  to compile the system commands.  (An old version of gcc/gfortran.)

```

```
Question: I am compiling with Intel Compilers and amd getting undefined references.

Answer:
  If you get an error message to a "ompc" routine like

      Ncpu.c:(.text+0x24): undefined reference to `ompc_set_num_threads'

  This is because you are using an old Intel compiler.  The makefile uses the
current option to enable OpenMP rather than the old option.

```

```
Question: Why don't the get_*.sh work?

Answer:
   The get_*.sh scripts download tables from the WMO github servers,
and local tables from various centers.  My work machine has wget v1.14,
and gets this error.

    wget: unable to resolve host address ‘-o-’

My home machine has wget v1.20 and works.  The created *.dat file can
be used on machines with an old wget.

```

```
  Question: how to compile on WCOSS-2?

   module install intel/19.1.3.304    module load the intel compiler
   export CC=icc
   export FC=ifort
   make                               this will fail because of missing libraries
   cp lib64/*a lib/                   they changed configure to write the libraries to lib64 instead of lib
   make

   OpenJPEG will not compile on WCOSS-2.  OpenJPEG requires the png, tiff, lcms, z libraries
   and the WCOSS2 build complains about missing libjbig and liblzma.  I see them in /usr/lib64.
   Icc/ifort will compile OpenJPEG on my personal machine, I spent enough time with this
   weird machine.

```

|

|     |
| --- |

|

---

|
| [NOAA/](https://www.noaa.gov/)
[National Weather Service](https://www.nws.noaa.gov/)
[National Centers for Environmental Prediction](https://www.ncep.noaa.gov/)
Climate Prediction Center
5830 University Research Court
College Park, Maryland 20740
[Climate Prediction Center Web Team](/comment-form.md)
Page modified: Dec 29, 2017, Oct 13, 2019, Sep 30, 2020, Oct 2020, Feb 2021, Oct 2021, Jan 2023.
| [Disclaimer](https://weather.gov/disclaimer.php) | [Privacy Policy](https://weather.gov/privacy.php) |

|

```

```

---

> Description: compile questions wgrib2 v3.0.2+

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/compile_questions.html>_
