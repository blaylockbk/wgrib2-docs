# Release Notes

# v3.1.1 (April 14, 2022)

Wgrib2 v3.1.1 is a minor release that addresses problems with

1. check_pdt_size causes fatal error for some ECMWF and ICON files
   work around for older versions: -check_pdt_size 0
1. -unix_time fails with new glibc (random failures)
   work around for older versions: $TZ should not be UTC, or a daylight saving time

## Highlights

Fixed check_pdt_size: added more PDTs, support for vertical coordinates

Fixed and updated: -unix_time failed with new glibc

- -unix_time: converted from posix to C89 code, and no longer optional
- -set_date: now understands unix time (seconds after start of Jan 1, 1970)
- -import_netcdf: display unix time codes in YYYYMMDDHH(mmss) format
- many minor changes

ECMWF ensemble files: if code table 4.7 undefined, set to reasonable value

-set_pdt: major changes, handles more PDTs, much better

Early testing of Netcdf4 compiled with Ubuntu 20.04LTS is promising.

# v3.1.0 (October 7, 2021)

**Big changes...many inventory changes**

wgrib2 upto v3.0.2 used NCEP web pages for table descriptions (scripts by M. Schwarb)

wgrib2 v3.1.0+ uses the WMO github for table descriptions (except field names) (scripts by M. Schwarb)

updated NCEP, ECMWF and DWD field names

old inv: :(chemical name): new inv: :chemical=(chemical name):

old inv: :process (number): new inv: :process=(number):

previous two changes are to make inventory consistent with -set_metadata

more compatibility for -set_metadata (wgrib2 file >inv; wgrib2 file -set_metadata inv)

## Fix for PNG decoder

Can now handle PNG compressed grib files that use a bit depth of 1, 2 and 4

Previously library could only handle 8 and 16 bit depths

## Other Changes

Public release source code, wgrib2.tgz, is now compatible with a BSD-varient tar

-reset_delayed_error: remove writes to stdout

-ndates: removed limit on size of output

nearest neighbor for global Gaussian grid is now a calculation rather than search

bug fix in -import_grib and -import_grib_fs when main input is an incorrectly encoded NCEP constant field,

the message read by -import_grib and -import_grib_fs will be incorrectly fixed.

# v3.0.2 (March 1, 2021)

Note that the file server (ftp.cpc.ncep.noaa.gov) had the wrong files for v3.0.1. The source code was for a older beta version. So wgrib2 v3.0.1 has been erased from history. Wgrib2 v3.0.2 is basically v3.0.1 plus with a minor upgrade to "extended names". The extended names can now optionally include the level and and forecast time information. This change allows -netcdf to produce unique variable names for fields that previously would have had the same name.

Wgrib2 v3.0.2 is a minor upgrade to v3.0.0. Rather than accumulating updates and new features for 12+ months, v3.0.1 is a relatively quick update that is needed for gnu make v4.3, and for providing initial python support on Windows (pywgrib2, cygwin). So that was the plan.

I had deferred adding D Jovic's code for using the OpenJPEG library for supporting jpeg2000 until the release of wgrib2 v3.0.0. It is now a compile time option.

M Schwarb sent me scripts that would read the ECMWF grib tables from the web and convert them to a format suitable for wgrib2. This lead to adding the -names option and adding the DWD grib tables. Of course, a wgrib2 update is not finished unless there several new options.

G Trojan worked on the fatal error handler, and that resulted in the removal of several routines, and cleaner code.

Delayed fatal errors were added. This allows wgrib2 to flag an error, and error out at the end of processing of the grib message. This allows the user to "debug" the grib message which caused the fatal error. In addition, delayed fatal errors can be converted to warnings.

For python, wgrib2 has options to read and write grib sections.

New compilers, means new warning and error messages. Fixed an error that would be triggered if your keyboard generated characters with the high bit (128) set and your compiler defines char as signed char.

Added support for Nvidia compilers from the Nvidia HPC SDK for building the wgrib2 executable and shared library. Previous versions of the Nvidia compilers (old branding: Portland) had problems compiling wgrib2.

Added support for AOCC to compile a shared library for pywgrib2.

Added support for cygwin (Windows) to compile a shared library for pywgrib2. Unfortunately the shared library does not support jpeg2000 or png compression because they use the zlib which refers to an undefined routine.

Fixed -new_grid with interpolation ot user-specified locations. I forgot to set the discpline of GEOLAT/GEOLON.

An update to ncep grib tables.

# v3.0.1 (February 24, 2021)

# v3.0.0 (September 2020)

**Wgrib2 gets a new numbering convention (XX.YY.ZZ).**

|     |                                                                        |
| --- | ---------------------------------------------------------------------- |
| XX  | incremented with major source code changes (infrequently)              |
| YY  | incremented with software code changes (approximately yearly releases) |
| ZZ  | incremented with minor code releases                                   |

Normally wgrib2 gets updated annually. It ususally takes that long to get enough updates to make it worthwhile for an upgrade. Of course, some users may need that latest feature and need the latest ZZ release. The schedule of the annual releases can vary widely. The expected Feb 2020 release was delayed to September 2020 to incorporate a python interface.

## Hightlights

The upgrade from v2.0.8 was deemed a major release because of the new IF-block structure and the shared library for python. Scripts written with the new IF block structure are not downward compatible, and I think that the python interface is a big deal.

- A real if/elseif/else/endif structure
- -import_grib_fs, import grib2 that matches a text string
- -new_grid location, interpolate to specified locations
- -new_grid_format: can write interpolated fields to grib, binary or ieee
- support of AOCC (AMD Optimizing C Compiler), based on clang/flang
- updated grib variable table
- fixed and increased functionality of -import_netcdf
- -ens_processing: do not use v2.0.8, major problem with some versions of gcc
- added spectral interpolation to -new_grid
- support making shared library (gnu compilers under linux and MacOS)
- support for python interface (linux, MacOS)
  
Wesley Ebisuzaki, 5/4/2020, revised 9/22/2020, 11/13/2020
