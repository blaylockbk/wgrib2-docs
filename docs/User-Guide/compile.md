# Compile from source

These instructions are for compiling wgrib2 `v3.0.2+`. 

[Download latest via HTTP](https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/wgrib2.tgz){ .md-button }

You can find older version [here](https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/).

<!-- [Download latest via FTP](ftp://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/wgrib2.tgz){ .md-button } -->


1. Download wgrib2 source code.
1. If it exists, remove the pre-existing `grib2/` directory.
1. Untar the `wgrib2.tgz` file.
1. Change into the newly created `grib2/` directory

```bash
wget https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/wgrib2.tgz
rm -rf grib2
tar -xzvf wgrib2.tgz
cd grib2
```

The makefile uses two and one optional environment variables that have to be set. With wgrib2 `v3.0.2+`, you need to set `$CC` and `$FC`, and the makefile tries to identify `$COMP_SYS` from `uname -a`. However, if the makefile cannot identify your system, `$COMP_SYS` will not be set. If your compilers are _gcc_ and _gfortran_, you can try compiling wgrib2 with `COMP_SYS=gnu_linux`.

| Computer System                            | Environment Variables                                 |
| ------------------------------------------ | ----------------------------------------------------- |
| linux, bsd-type OS, gcc/gfortran compilers | `COMP_SYS=gnu_linux`<br>`CC=gcc`<br>`FC=gfortran`     |
| linux, AOCC                                | `COMP_SYS=clang_linux`<br>`CC=clang`<br>`FC=flang`    |
| linux, icc and ifort                       | `COMP_SYS=intel_linux`<br>`CC=icc`<br>`FC=ifort`      |
| linux, icx and ifx                         | `COMP_SYS=oneapi_linux`<br>`CC=icx`<br>`FC=ifx`       |
| linux, Nvidia HPC SDK                      | `COMP_SYS=nvidia_linux`<br>`CC=nvc`<br>`FC=nvfortran` |
| Windows, cygwin gcc and gfortran           | `COMP_SYS=cygwin_win`<br>`CC=gcc`<br>`FC=gfortran`    |
| MacOS, real gcc and gfortran               | `COMP_SYS=gnu_mac`<br>`CC=gcc`<br>`FC=gfortran`       |

!!! warning "Not recently tested AIX, gnu_linux_g95, and open64"

The below is an example of setting environment variables and running make in a **BASH** shell.

```bash
export CC=gcc
export FC=gfortran
export COMP_SYS=gnu_linux

make        # to make wgrib2
make lib    # to make wgrib2 library
```

> ⚠️ Note: If `COMP_SYS` is not defined, the makefile will attempt to determine the `COMP_SYS`.

> ⚠️ Note: For NCEP's WCOSS-2. see the question on lib64, and WCOSS-2.

## Intel Compilers

The Intel C compiler will not compile the Jasper library (jpeg2000 support). To get jpeg2000 support, you will either have to use OpenJPEG or compile Jasper with the gnu compiler. The makefile will automatically use `gcc` when
trying to build the Jasper library when the classic Intel compilers are used.

To compile with the intel compilers on linux,

```bash
# Classic compilers
export CC=icc
export FC=ifort
export COMP_SYS=intel_linux
make
```

```bash
#LLVM compilers (wgrib2 v3.1.2+)
export CC=icx
export FC=ifx
export COMP_SYS=oneapi_linux
make
```

> Note: Compiling wgrib2 using the Intel compilers on Windows is possible but is not supported. (Grib files will be limited to 2 GB, and I have no way to test.)

## Compiling with Cygwin (Windows)

The only Windows C compiler supported is Cygwin's gcc.
The other Windows C compilers follow Microsoft's lead where
a "long int" is 32 bits on a 64-bit operating system. Wgrib2 will work with a 32-bit long int but will limit a grib file to barely acceptable 2 GB size. Cygwin's gcc also supports POSIX which means that you don't have to turn off features that require POSIX.

## Compiling in MacOS

You need to use compile with `gcc` and `gfortran`. The default MacOS installation has gcc pointing to clang. You can get the real deal from homebrew.

With wgrib2 v3.0.0, MacOS support is now builtin. For prior
releases, there have been nice pages which detail the compiling process.

## Compiling with other compilers

There is no support for other compilers. At one time, AIX was supported until our machines were scraped (2012). Use to support `gcc/g95` and the open64 compilers until development was stopped on g95 and open64. I haven't tried the Cray compilers because the Crays are already well supported by `gcc/gfortran`, `icc/ifort`
and `clang/flang`.

While there is no support for other compilers, the wgrib2 source code was written to the OS, compiler and hardware independent. The only limitation is that integers need to be 32+ bits long. Only compiler/hardware feature is an optional optimization to replace a loop with a `__builtin_clz()` which works when GNUC >= 4. The main difficulties with porting to another system should be in the libraries and disabling POSIX features.

# Makefile Options

There are many options that are documented in the makefile (see `grib2/makefile`).

- Options to remove features that require POSIX support.
- Options to remove libraries that are not public domain or not under a GNU license.
- Options for code that may be difficult to cross-compile.

## Python Support, make a shared library

Python support requires a shared wgrib2 library. This option only works for gnu/linux, gnu/MacOS, gnu/Windows, nvidia/linux AOCC/linux, icx/ifx.

Set the environment variable

```bash
MAKE_SHARED_LIB=1
```

then to make the shared library do

```bash
make clean
make lib
```

The shared library will be in `lib/`

## Grib names

With wgrib2 `v3.0.2+`, the grib variable names are shown using the DWD, ECMWF or NCEP tables. The locally defined variable names are shown by the table of the local center. This option sets the default grib table.

Not all names could be accommodated because some of the names had level, timing information. This feature is more beta because of the difficulty with level and timing information in the variable names.

```bash
USE_NAMES=NCEP   #  use NCEP name (default)
USE_NAMES=ECMWF  #  use ECMWF names
USE_NAMES=DWD1   #  use DWD names (DWD has two center ids)
```

## NetCDF

### Option 1: No NetCDF support

- fastest compile
- small executable (3.9 MB as of 2014)
- compile works on 32+ bit machines
- cannot read nor write NetCDF files
- no library conflicts in wgrib2lib when calling program
  uses netcdf or hdf
- makefile configuration: `USE_NETCDF3=0`, `USE_NETCDF4=0`

### Option 2: NetCDF3 support (default in makefile)

- fast compile
- modest increase in executable size (5.7 MB vs 3.9 MB as of 2014)
- compile may fail on 32-bit machines
- library conflict in wgrib2lib when calling program that uses netcdf
- makefile configuration: `USE_NETCDF3=1`, `USE_NETCDF4=0`

### Option 3: NetCDF4 compile libraries

- slow compile time (hours on an Intel Apollo Lake)
- 3.5x increase in executable size (13.5 MB vs 3.9 MB as of 2014)
- makefile configuration: `USE_NETCDF3=0`, `USE_NETCDF4=1`
- HDF5-1.10.4: compile works on RedHat 6 (gcc 4.x and intel 17.03)
- HDF5-1.10.4: compile has failed on Ubuntu and various other systems (newer gcc)
- HDF5-1.10.4: compile has failed on Ubuntu and AOCC (clang variant)
- HDF5-1.10.6: compiles on Ubuntu and many other machines
- HDF5-1.10.6: requires fortran90 compiler
- HDF5-1.10.6: wgrib2 fails with hdf5 internal error
- library conflict in wgrib2lib when calling program that use netcdf
- makefile configuration: `USE_NETCDF3=0`, `USE_NETCDF4=1`
- prompts for downloading netcdf4/hdf5 libraries

### Option 4: NetCDF4 external libraries (v3.1.2+)

- User is responsible for validating the results using the netcdf4/hdf5 libraries.

The netCDF4 option is currently unsupported (10/2020). The previous release of hdf5 would not compile with modern gcc compilers. According to the release notes, the current release of hdf5 supports more modern version of gcc but not the latest versions. In personal testing, I had to revert to an older version of hdf5 to make wgrib2 work. It may be a problem with my code. However, the older version will not compile with even a moderately modern gnu compiler. Older versions of ihe Intel compiler were fine, but the latest version had problems (1/2021). Preliminary testing of wgrib2 v3.1.1 was promising for netcdf4.

Probably every OS includes precompiled netcdf4 and hdf5 libraries. Modifying the wgrib2 makefile to use the system netcdf4 files will depend on the system and
the source of the netcdf4 libraries.

# JPEG2000: Jasper, OpenJPEG

With wgrib2 `v3.0.0+`, jpeg2000 compression can be handled by either the Jasper or OpenJPEG library. Both libraries are equally slow and files are roughly the same size.

**Jasper** The advantages with the Jasper library is that the last problem with the Jasper library was fixed
may years ago. The disadvantage with Jasper is that support may be lacking and it doesn't compile with several C compilers.

**OpenJPEG** The advantage with OpenJPEG is the support is ongoing. The disadvantage is that it requires cmake to build. Consequently I cannot build and test the OpenJPEG version on my linux workstation at work. Eventually OpenJPEG will be the default option.
