  wgrib2: wgrib for GRIB-2 
 Utility to read and write grib2 files 
News

[wgrib2 v3.1.1 has been released](./wgrib2_v3.1.1_changes.html)

News

[pywgrib2\_s](./pywgrib2_s.html) been released for beta (Linux/MacOS) (wgrib2 v3.0.0, 9/2020)

Introduction
 Wgrib2 is a processor for grib2 files. It is a utility
and library for manipulating grib files, The utility was designed to be
used to reduce the need for custom Fortran programs to 
read, write and manipulate grib files.
Wgrib2 has the following abilities.

* inventory and read grib2 files
* create subsets
* create regional subsets by cookie cutter or projections
* export to ieee, text, binary, CSV, netcdf and mysql
* import to ieee, text, binary, and netcdf
* write of new grib2 fields
* parallel processing by using threads (OpenMP)
* parallel processing by flow-based programming


 Wgrib2 is versatile because it's command line is a simple
language. This makes wgrib2 useful in embedding. 
Some programs that embed wgrib2.

* [g2ctl](https://www.cpc.ncep.noaa.gov/products/wesley/g2ctl.html)
a control maker for [GraADS](https://www.nws.noaa.gov/cgi-bin/nwsexit.pl?url=https://www.iges.org/grads/)* [atl\_g2ctl, alt\_gmp](https://www.cpc.ncep.noaa.gov/products/wesley/alt_g2ctl_gmp.html)
alternative versions of g2ctl/gribmap for for
[GraADS](https://www.nws.noaa.gov/cgi-bin/nwsexit.pl?url=https://www.iges.org/grads/)* grib-filter/g2subset cgi-bin programs for Nomads (NOAA OperationalModel Archive & Distribution System)
* [g2grb.gs](https://www.cpc.ncep.noaa.gov/products/wesley/g2grb.html) enables GrADS to write grib2 files
* [grb1to2.pl](https://www.cpc.ncep.noaa.gov/products/wesley/grb1to2.html) grib1 to grib2 converter
* [Xplane11](https://www.x-plane.com/) flight simulator
 * [rNOMADS](https://www.nws.noaa.gov/cgi-bin/nwsexit.pl?url=https://cran.r-project.org/web/packages/rNOMADS/index.html) R interface for NOAA weather data



OpenMP Configuration


OpenMP is used to speed up wgrib2 by running loops over
multiple cores. OpenMP is enabled by default for most builds
and you can slow up your machine if you the wrong OpenMP
configuration.

The first configuration is to set up the number of
threads that wgrib2 will use. The default configuration
is set the number of threads equal to the number of
physical cores. The default is reasonable except for
when your computer becommes becomes
unresonsive. For a 4 core CPU, I set the number of threads
to be 3, so I another core to handle other work.


```

export OMP_NUM_THREADS=3

```

The next OpenMP configuration is how to handle unused threads (cores).
By setting OMP\_WAIT\_POLICY to PASSIVE,
unused cores are made availible to to work on other tasks.
Setting OMP\_WAIT\_POLICY to ACTIVE, will not share the cores.
In kindergarten, you learned the value of sharing.


```

export OMP_WAIT_POLICY=PASSIVE

```

News

4/14/2022: wgrib2 v3.1.1 is released
 Changes for wgrib2 v3.1.1
* fixes check\_pdt\_len for some ECMWF and ICON files with vertial coordinates
* fixes -unix\_time because of glibc
* more support for unix time: -set\_date, -import\_netcdf
* tested: gcc/gfortran on Ubuntu 20.04, Redhat 7
* tested: icc/ifort on Redhat 7
* tested: AOCC v3.2 (clang, flang) on Ubuntu 20.04
* minor tested: Windows 10 using cygwin64
* minor tested: gcc/gfortran on Ubuntu 18.04


Usage
* each option corresponds to a subroutine call
* type wgrib2 to see primary options
* inventory format is specified on command line by options
* if no "inv" option is specified, -s is used


Joining the development effort
* Source code: knowledge of C and some grib-2
* Changes to existing source code has to use the same licence as the original code
* New code (files) must either be GNU or public domain.
* Github? No. 
* Bug reports are important
* Some inovations by first time contributors
	+ first implementation of lat-lon of common grids
	+ write netcdf files
	+ callable wgrib2 (making wgrib2 a subroutine)
	+ write to mysql files
	+ AEC compression
	+ python interface* Contact wesley.ebisuzaki@noaa.gov


Contributions by
* Wesley Ebisuzaki: many modules
* Reinoud Bokhorst: tosubmsg, checksum 
* DWD (Gregor Schee, Daniel Lee and others): AEC compression
* Jaakko Hyvätti: gribtab
* John Howard: callable wgrib2
* Dusan Jovic: staggered grids, proj4 code
* Kristian Nilssen: netcdf module
* Karl Pfeiffer: georeferencing
* Pablo Romero: unix\_time
* Manfred Schwarb: many modules
* Arlindo da Silva: openGrADS, bbox
* Niklas Sondell: mysql module
* Sam Trahan: satellite tables
* George Trojan: python interface, improvements to wgrib2api
* Sergey Varlamov: netcdf module improvements, georeferencing updates
* thanks to the people who report the bugs and more who provide the fixes!


Documentation
* [problems-error message](./problems.html)* [Slides for talk to CUNY students 6/2016: grib1/grib2](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/grib1-grib2v3.pptx)* [intro\_grib2.pdf using GFS fcst](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/intro_grib2.pdf)* [Understanding the default inventory](./default_inv.html)* [converting from wgrib to wgrib2 in scripts](./convert_wgrib2.html)* [Powerpoint presentation](https://www.cpc.ncep.noaa.gov/products/outreach/seminars/CPC/archive/grib2_wgrib2.ppt) 4/6/2011 at NCEP 
* [Changes in wgrib2 from 5/2012 to 1/2015 (pdf)](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/wgrib2_changes_2012-2015.pdf)* [submessages](./submessages.html)* [writing a simple function](./function.html)* [selecting fields to decode](./selecting_messages.html)* [option types: init inv out misc](./types.html)* [bin, ieee, text format](./bin_ieee_text_format.html)* [small fast databases](./small_fast_databases.html)* [usage questions](./usage_questions.html)* [using pipes](./pipes.html) and [multiprocessing](./for_n.html)* [compile questions](./compile_questions.html)* [limitations](./limitations.html)* [make wgrib2 faster](./speed.html)* [calling wgrib2 from C](./calling_wgrib2.html)* [user defined grib tables](./user_grib2tables.html)* [special files](./special_file_names.html)* [some tricks](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/tricks.wgrib2)* [some tricks for NCEP users](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/tricks.ncep)* [one-line tricks](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/tricks.cheap)


The options
* [the common options](./short_cmd_list.html)* [all the commands](./long_cmd_list.html)


Some solutions
* [Time interpolation of two grib files](./time_interpolation.html)* interpolation to new grid: [new\_grid](./new_grid.html), [Lat-Lon nearest neighbor](./lola.html), 
[Lat-Lon by Cressman](./cress_lola.html)* CSV (comman separated value) file: [-csv](./csv.html), [-spread](./spread.html)* [Windows\_10 version of wgrib2](https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/Windows10/)* [Fast averaging](./ave.html)


Selecting Fields/Records/Message
Select/Ignore by name/level/time/etc
* [-match](./match.html) process records that match a posix extended regular expression
* [-not](./not.html) process records not matching a regular expression
* [-match\_inv](./match_inv.html) the inventory used by -match and -not
* [-i](./i.html) reads inventory from stdin for record selection


Select by number (better to use previous method)
* [-d](./d.html) dump specific record
* [-for](./for.html) select a range of records to process (nth message)
* [-for\_n](./for_n.html) select a range of records to process (nth message/submessage)


Selective Processing: if options

After selecting the fields to process, you can refine the processing by
the "if" options. With wgrib2 v3.0.0, the [IF](./if.html)
structure was improved. Scripts that used the older IF structure still work;
however, the new IF structure is easier to read.

If possible, it is better to use the match options than the if options.
The -match/-not options prevent unwanted records from being processed which
saves time over the if options which process the fields.

Individual Grid Point Data
* [-ij](./ij.html) print value at grid point (i, j)
* [-ijlat](./ijlat.html) print lat, lon, value at grid point (i, j)
* [-ilat](./ilat.html) print lat, lon, value of Nth grid point
* [-lon](./lon.html) prints the lat-lon, and value of the grid point nearest the specified lat-lon
* [-max](./stats.html) prints the maximum value
* [-min](./stats.html) prints the minimum value
* [-stats](./stats.html) prints some statistics about the fields
* [-V](./macros.html) verbose inventory (shows stats)


Regridding, Interpolating to new grids
 Wgrib2 has the ability to convert grib files from one grid
to another. The conversion uses a user-selected interpolation
scheme: bilinear (default), bicubic, nearest neighbor, budget, and spectral.
The supported grids include lat-lon, gaussian, Lambert conformal, polar stereographic,
and WMO-defined rotated lat-lon grid. This capability uses the NCEP IPOLATES2 library
and is an optional package. 

 Note: the interpolation uses scalar and vector interpolation
schemes. For the vector quantities the V field must immediately
follow the corresponding U field.

* [new\_grid](./new_grid.html)* [-new\_grid\_interpolation](./new_grid_interpolation.html) set interpolation type used by -new\_grid
* [wgrib2m](./wgrib2m.html) fast regrid using multiple processes


Exporting data to other programs
* [-netcdf](./netcdf.html): write data in netcdf format
* -mysql: export data to a mysql database
* -mysql\_speed: export data to a mysql database
* [-spread](./spread.html): write data for spreadsheets
* [-csv](./csv.html): write in column separated values, another one for spreadsheets
* [-text](./text.html): data in text format
* [-bin](./bin.html): data native binary floating point
* [-ieee](./ieee.html): data in big endian IEEE format
* [-ijbox](./ijbox.html): write a rectangular grid of data
* [-AAIG](./AAIG.html): arcinfo ascii grid, GIS



For a short list of options, type "wgrib -h"
For a complete list, type "wgrib -help all"
To search for an option, type "wgrib -help keyword"
Writing grib2

Wgrib2 has adopted the template approach for writing grib. You
have a sample grib2 message (template), and you modify the grid point values
and metadata to create a new grib message that you can write.
This is similar to how ECMWF's ECCodes writes grib. The other approach
is to supply many parameters to create a grib message which is used
by NCEP's g2 library.

1. from wgrib2 [command line](./gribify2.html)- from GrADS using [g2grb.gs](../g2grb.html)- fron python using [write](./pywgrib2_s_write.html)- from fortran using [grb2\_write](./grb2_wrt.html)


Machines able to run wgrib2
 64-bit with IPOLATES
* Redhat 7 Enterprise: gcc/gfortran (primary development system)
* Redhat 7 Enterprise: gcc/gfortran, icc/ifortran
* SUSE Enterprise: gcc/gfortran, icc/ifortran
* Ubuntu 20.04: gcc/gfortran (primary development system)
* Ubuntu 20.04: AOCC's clang and flang (development system) with OpenJPEG
* Ubuntu 20.04: nvidia with OpenJPEG
* ARM: needed to be compiled with USE\_NETCDF4=1, USE\_JASPER=0 (old report)
* Redhat linux: 32-bit with IPOLATES, not tested recently, use netcdf4 (old report)
* Mandriva linux (old report)
* AIX: use makefile, some fiddling with libraries is necessary, not tested recently
* Solaris, needs gnu make and gcc (old report)
* Solaris-10 (old report)
* HPUX, needs changes to makefiles (old report)
* Windows: using Cygwin system produces 32-bit binaries (old report)
* Windows: using Cygwin system produces 64-bit binaries
* Windows: compiled MingW (not recent), Watcom C, icc/ifort (old report)
* Windows/linux subsystem (ubuntu): compiled with gcc/gfortran (old report)
* Intel-based Mac using gcc and gfortran



The makefile works on Redhat and Ubuntu (with needed installed options).
For other systems, you may have to modify the makefile. The makefile
requires gnu make which is a common version of make.

System dependencies: 32 vs 64 bit, big vs little endian, Windows vs Linux/UNIX

The wgrib2 source code is written to be portable; there are no
issues with big vs little endian or the size of the integer as
long as it is 32 bits or more. The source code is written in
ANSI/ISO C (C89), with optional features that require
POSIX or POSIX-2. There has been a debate about 
moving the base rquirements to C99.

* big vs little endian: either works
* 32-bit machine: files limited to 2GB files, netcdf3 may not work
* Windows: 2GB+ files with 64-bit cygwin, 2GB with other windows C compilers
* Windows: only 64-bit cygwin is supported
* POSIX: all POSIX code is optional. Regex support is POSIX and useful.
* JPEG2000: OpenJPEG library or Jasper library


Source Code and Compling Hints

The wgrib2 source code is written to the POSIX-2 standard. Features requiring
POSIX2, such as regular expressions, can usually be turned off
in the makefile. The wgrib2 code can be compiled with 32 or 64 bit pointers and integers.
However, the code has to be compiled in a like manner for all the libraries.
Some packages are optional (netcdf, mysql) and enabling these options can really
increase the executable size. 


While compiling wgrib2, you may see warnings about unknown pragmas. Pragmas
are "comments" that are used when compiling an OpenMP version of the code. The
default is to compile with OpenMP turned on.


2/2019: The HDF5 library has problems being compiled with newer gcc compilers.
The only consisten method of adding NetCDF4 to wgrib2 is by using old compilers
(RedHat 6) or using the Intel compilers. The work around to enable NetCDF3 support
and use an external utility to convert the files to Netcdf4.


NetCDF3 has problem being compiled on 32-bit machines.

* [Source code](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/)* [\_README](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/_README)* [Change log](https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/Changes)* [compiling directions and questions](./compile_questions.html)* [wgrib2.tgz](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/wgrib2.tgz)* [OS-X installation by author of rNOMADS](https://bovineaerospace.wordpress.com/2017/08/20/how-to-install-wgrib2-in-osx/) using gcc and OS-X


Precompiled code from External Sites

There are many sites with precompiled versions of wgrib2. This list
is neither exhaustive nor an endorsement of the sites. I have
not tested the wgrib2 executables from these sites and YMMV.

* [OpenGrads:](https://www.nws.noaa.gov/cgi-bin/nwsexit.pl?url=https://opengrads.org/)
Darwin, Freebsd, Linux, Windows(cygwin)
* [Fedora Project](https://www.nws.noaa.gov/cgi-bin/nwsexit.pl?url=https://fedoraproject.org)* [RPMs:](https://www.nws.noaa.gov/cgi-bin/nwsexit.pl?url=https://download.opensuse.org/repositories/home:/gbvalor/) Centros, Fedora, OpenSUSE, RedHat, SUSE
* [MacPorts](https://www.nws.noaa.gov/cgi-bin/nwsexit.pl?url=https://trac.macports.org/browser/trunk/dports/science/wgrib2/Portfile)


Status

The wgrib2 is used operationally in the NCEP production suite.


[Change logfile](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/Changes)

Bugs
 Please report bugs to wesley.ebisuzaki@noaa.gov. When you report bugs,
try to make them reproducible on a linux machine and include sample data.

Distribution

 The source code modules for wgrib2 are either in the public domain or under the GNU
 licence depending on the authors of the various modules. Wgrib2 uses libraries that
 are in the public domain, under various GNU licences, the Image Power JPEG-2000 Public Licence\*, 
 libpng licence\*, the zlib licence\*, the netcdf licence\*, HDF5 licence\*, MySQL licence\* and perhaps others. How 
 about one licence to rule them all?
 
 \* optional package
 
 |