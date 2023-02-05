# Compiling Questions

## Some libraries are being created in grib2/lib64

Normally the libraries created by the wgrib2 build are stored in grib2/lib. However, I found one machine that put some libraries in grib2/lib64. One builds many of the libraries by first configuring the build by running the "./configure" program. The "./configure -help" command lists the various values. However, if `$CONFIG_SITE` is defined, the `$CONFIG_SITE` script can change can alter the default values or even more. In my case, you can work around this `$CONFIG_SITE` by

```bash
make   # (crashes)

cp -r lib64/* lib/

make   # (builds wgrib2/wgrib2)
```

## Missing zlib.h

I haven't been able to reproduce this compiling problem. The png compression requires both the zlib library and the zlib.h include. I suspect that the libpng configure program is using the system zlib because it finds the system zlib library. Perhaps the fix is to install the zlib.h files by installing the "zlib-develop" package. Another fix is to remove the system zlib and let libpng use the wgrib2-supplied zlib. The third fix is to diable png compression in the make file. PNG compression is not very good and I have only seen it used by RADAR files from ESRL.

## Other Questions

### Warnings about parameter type mismatch in fftpack.f"?

The file `fftpack.f` is was written to an old fortran standard before you had an allocatable array and memory was at a premium. To save space, a real work array was defined and used in some parts of the code. The same array was used in other parts of the code which needed an integer work array. You save memory by using the same array in both places. It was common practice and the old compilers didn't complain. Ignore the warnings.

### Why do you not support "make install"?

"Make install" is not supported by the makefile because
installation is just copying the executable to another location. There is no obvious default for all systems.

### Why do you build japser with the following flags?

```
--disable-libjpeg --disable-opengl
```

Libjpeg is not needed by wgrib2. The makefile should work when the system doesn't have libjpeg installed.

Opengl is not needed by wgrib2. Makefile should still work when the system doesn't have opengl installed.

The wgrib2 makefile compiles the libraries in a manner optimized for wgrib2. The libraries are not meant for general use. For example, the netcdf library doesn't have a fortran interface because I want the makefile to work if a user lacks a fortran compiler.

### Why do build zlib and libpng?

Not all linux distributions include libpng or they give it a different name. Some linux distributions may have different versions of libpng and zlib.

### Why don't you make netcdf4 the default netcdf package?

The Netcdf4/hdf5 libraries are very big and the end result is a long download time and a large executable. If you decide to compile with Netcdf4, the make will fail with the commands needed to download the netcdf4 and hdf5 libraries. After you have download the libraries, the make should work.

At the current time (9/2020), compiling with the netcdf4 option has been unsupported for a few years. (Classes of users will have problems and nothing can be done.) Until recently, the hdf5 library had problems being compiled with the newer versions of gcc. The latest version will compile with the gcc included with Ubuntu. However the newest version breaks the `-import_netcdf` option with a uninformative hdf5 error message.

### Why don't you automatically download the netcdf and hdf5 packages?

People concerned about security would not like that action.

### I get the error message: `/bin/sh: ./configure: Permission denied`

It is possible to configure a filesystem so that the execute bit is set to zero. Programs and scripts will not execute when they reside on that filesystem. Try compiling on another filesystem. If that doesn't work, you are probably not allowed to download programs. The solution is to ask that your sys admin to compile the program for you. But if your sys admin will compile code for you, why are you reading this page? Let him read this page.

### Why the options to turn off aec, jpeg2000 and png compression

Some HPC machines use one type of cpu and/or OS to compile and another type of cpu and/or OS to run (cross-compile). Using configure scripts can be a challenge when cross-compiling. I wanted a reasonable wgrib2-subset that can be cross-compiled. Consequently all libraries that that require a configure script are optional.

The configure programs of various libraries may not handle certain machines or compilers. For example an ARM user had to turn off the jasper library. Clang on the MacOS may be a problem for the AEC library.

### Why does compiling wgrib2 with the Netcdf option fail?

This frequently happens with 32-bit machines. This problem goes away on a 64-bit platform or if netcdf-4 is used instead of netcdf-3. This problem is not being addressed because of a lack of 32-bit machines.

Some versions of hdf5 will not compile with modern C compilers.

### Why the option to turn off g2clib?

g2clib is not needed but it can be installed for testing at NCEP.

### Why gctpc and proj4? Why not proj4 without gctpc?

Gctpc and proj4 are both projection libraries and only one is really needed. Proj4 has more functionality, is modern and has an active support group. Gctpc is old and is more-or-less unsupported. On the other hand, gctpc is simpler, works and supports OpenMP like the rest of wgrib2. Proj4 supports a different threading model. As grids get larger, you want the speed of gctpc. Both libraries are being supported in order to help debugging and in the case that a future grid requires Proj4 support.

### Why so many compile options?

Some systems or compilers do not support certain POSIX features Some functionality is not needed by most systems. Not all systems have fortran, MySQL, or support NetCDF/HDF5. The code that calls IPOLATES is not using a standard interface. Some systems do not handle cross-compiling easily. Some uses of wgrib2 may require a small RAM usage. Same makefile is used to compile the wgrib2 library. Some compilers may not support C99 or C11 (future).

Problem: I compiled wgrib2 and when I run it, it complains about missing libraries.

I have the same problem with our Cray. If you compile with the Intel compilers, you have to load the Intel environment before running the executables. If you compile with the GNU compilers, you have to load the GNU environment before running. The way to get around this problem is to compile wgrib2 with the same compiler that was used to compile the system commands. (An old version of gcc/gfortran.)

### I am compiling with Intel Compilers and amd getting undefined references.

If you get an error message to a "ompc" routine like

```
Ncpu.c:(.text+0x24): undefined reference to `ompc_set_num_threads'
```

This is because you are using an old Intel compiler. The makefile uses the current option to enable OpenMP rather than the old option.

### Why don't the `get_*.sh` work?

The get\_\*.sh scripts download tables from the WMO github servers, and local tables from various centers. My work machine has wget v1.14, and gets this error.

    wget: unable to resolve host address ‘-o-’

My home machine has wget v1.20 and works. The created \*.dat file can be used on machines with an old wget.

### How to compile on WCOSS-2?

```bash
module install intel/19.1.3.304 module load the intel compiler
export CC=icc
export FC=ifort  # make this will fail because of missing libraries
cp lib64/*a lib/ # they changed configure to write the libraries to lib64 instead of lib
make
```

OpenJPEG will not compile on WCOSS-2. OpenJPEG requires the png, tiff, lcms, z libraries and the WCOSS2 build complains about missing libjbig and liblzma. I see them in `/usr/lib64`. Icc/ifort will compile OpenJPEG on my personal machine, I spent enough time with this weird machine.

---

> Description: compile questions wgrib2 v3.0.2+

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/compile_questions.html>_
