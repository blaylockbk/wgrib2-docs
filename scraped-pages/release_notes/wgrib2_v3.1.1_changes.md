
### wgrib2: v3.1.1



### Changes from wgrib2 v3.1.0 to v3.1.1


 Wgrib2 v3.1.1 is a minor release that addresses problems with

1. check\_pdt\_size causes fatal error for some ECMWF and ICON files  

work around for older versions: -check\_pdt\_size 0
- -unix\_time fails with new glibc (random failures)  

work around for older versions: $TZ should not be UTC, or a daylight saving time


 Highlights
2. fixed check\_pdt\_size: added more PDTs, support for vertical coordinates
- fixed and updated: -unix\_time failed with new glibc
	* -unix\_time: converted from posix to C89 code, and no longer optional
	* -set\_date: now understands unix time (seconds after start of Jan 1, 1970)
	* -import\_netcdf: display unix time codes in YYYYMMDDHH(mmss) format
	* many minor changes- ECMWF ensemble files: if code table 4.7 undefined, set to reasonable value
- -set\_pdt: major changes, handles more PDTs, much better
- Early testing of Netcdf4 compiled with Ubuntu 20.04LTS is promising.


### Future Changes for wgrib2 v3.1.2



Added better support for grids up to 2\*\*32-1 (4,294,967,295)
grid points. This endeavor started in 2016. Progress
started slowly because of the lack of large-memory machines and urgency.
However, I received my first bug report about the 4G problem.

Cmake is required for latest libaec. So the makefile is now
cmake aware. You can build wgrib2 without cmake but you will not be
able to install libaec or OpenJPEG.

The parallelization of the uncompressing of complex-compressed files
was improved. Decoding of the GFS master file went from 14 seconds
to 9 seconds on a 6-core ryzen 5600g cpu using a nmve drive. Unfortunately
the speed was slow and unimproved on a Luster file system. 

Previous versions of wgrib2 handled non-spherical Lambert Azimuthal Equal Area Projection 
using a spherical earth. Now this projection is handled by proj4 which means that
proj4 is now installed by default.

The geolocation tag (-geolocation) is better defined (external added). Output is
changed from "XYZ" to "geolocation=XYZ".

Alpha: USE\_NETCDF4 redefined and USE\_HDF5 added. The change
allows linking to precompiled NetCDF/HDF5 libraries.

Alpha: compiling with Intel's oneAPP icx and ifx compilers.
Icx had a problem executing one OpenMP loop. Either
the loop is incorrect and several compilers have been
accidently generating "correct" code for years, or icx is
silently generating bad code. Anyways this is why wgrib2
compiled by icx/ifx is considered alpha status.

Update for local JMA product definition templates.





















----

>Description: 3.1.1 Changelog

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/wgrib2_v3.1.1_changes.html>_