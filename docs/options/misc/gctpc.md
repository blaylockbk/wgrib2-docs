
### wgrib2: -gctpc, -proj4



### Introduction



Wgrib2 has 3 sets of geolocation routines. Geolocation routines
are used to find the locations of the grid points by a (i,j) -> (lon,lat)
transformation. Some geolocation routines can calculate (i,j) from (lon,lat).
There is no "best" set of routines, so up to 3 sets can be used.


Internal Routines
1. Spherical Earth only
- Very fast and multithreaded, 3.5/12.7 sec on test case 1\*
- Very fast and multithreaded, 3.5/12.7 sec on test case 2\*
- Needed for NWP grids such as Gaussian grids and rotated lat-lon
- Common projections
- C, no configure file needed
- Can transform (i,j) to (lon,lat)
- Many of the codes trace their origins to operational codes at NCEP



GCTPC library
1. Handles ellipsoidal Earth
- Slow and multithreaded (OpenMP), 5.5/19.5 sec on test case 1\*
- Slow and multithreaded (OpenMP), 15.7/58.7 sec on test case 2\*
- More projections than internal routines
- C, no configure file needed
- Can transform (i,j) to (lon,lat)
- Can transform (X,Y) to (lon,lat)
- Can transform (lon,lat) to (X, Y)
- Library is old but working, no recent development
- Support for library is unknown



Proj4 library
1. Handles ellipsoidal Earth
- Slow and no support for OpenMP, 17.8/19.7 sec on test case 1\*
- Slow and no support for OpenMP, 62.8/64.7 sec on test case 2\*
- Proj4 supports pthreads, wgrib2 uses OpenMP
- More projections than GCTPC, ex. ellipsoidal Lambert Azimuthal Equal Area grid
- C, requires config script
- Can transform (i,j) to (lon,lat)
- Can transform (X,Y) to (lon,lat)
- Can transform (lon,lat) to (X, Y)
- Library has active development
- Support for library is good
- The first choice by many people.



```

* test case 1 - read large Lambert conformal grid (6887 x 6610) and 
calculate lon/lat for the entire grid and print lon/lat for 1 grid point.
This uses a spherical Earth.

* test case 2 - read large Lambert conformal grid (6887 x 6610) and 
calculate lon/lat for the entire grid and print lon/lat for 1 grid point.
This uses a ellipsoidal Earth.

The timing code is

  time wgrib2 burned_area_20120131_12_23_1km.grib2.c0 -ijlat 4 3
  time wgrib2 burned_area_20120131_12_23_1km.grib2.c0 -ijlat 4 3 -gptpc 1
  time wgrib2 burned_area_20120131_12_23_1km.grib2.c0 -ijlat 4 3 -proj4 1

and the two numbers correspond to the wall clock and CPU times for a 4
core Intel Xeon E5506 with OMP_NUM_TREADS=4.

grid:
  Lambert Conformal: (6887 x 6610) input WE:NS output WE:SN res 0
  Lat1 64.997800 Lon1 180.492400 LoV 264.000000
  LatD 40.000000 Latin1 20.000000 Latin2 60.000000
  LatSP 0.000000 LonSP 0.000000
  North Pole (6887 x 6610) Dx 1000.000000 m Dy 1000.000000 m mode 0

Earth:
  case 1: code3.2=0 sphere predefined radius=6367470.0 m
  case 2: code3.2=4 IAG-GRS80 ave radius=6367444.5 m

Packing: grid point data - complex packing,c1 

```


The internal routines are fast, multithreaded but only handle 
a spherical earth and cannot transform from (lon,lat) -> (X,Y).
The internal routines include grids not included with GCTPC.


The GCTPC routines can handle an ellipsoidal Earth. They are
50% slower than the internal routines for a spherical earth and
are acceptable for NOMADs. For an ellipsoidal Earth, the speed is
probably adequate for NOMADS. The lack of support is not a major factor 
because the codes are tested and relatively simple. By supporting 
both GCTPC and Proj4, I am comfortable that the codes can be 
adequately tested.


Proj4 is the Gold standard; it used by many projects, it has
good support and updates appear on a regular basis. Proj4 supports 
more projections than GCTPC. Unfortunately Proj4 is slow. For a
spherical Earth, Proj4 is probably adequate (slower than gctpc
for a spherical earth). For an ellipsoidal Earth, Proj4 is
too slow for Nomads. There have been some efforts to make Proj4
thread safe. Another difficulty with Proj4 is that compiling
uses a config script. Config scripts can cause problems
when cross-compiling for compute nodes (some HPC computers).




Wgrib2 needs the internal routines for grids not supported by
GCTPC/Proj4. Wgrib2 needs GCTPC/Proj4 for handling ellipsoidal
Earths. The current policy is that Proj4 will be an optional
package and GCTPC will be come the default package in the 
near future. Support for Proj4 is useful for debugging and
for "keeping the options open".



### Usage




```

-gctpc 0
   do not use gctpc for geolocation
-gctpc 1
   enable gctpc for geolocation
-proj4 0
   do not use Proj4 for geolocation
-proj4 1
   enable Proj4 for geolocation

   Priority:
     Try Proj4 if Proj4 is enabled (not default) and installed (not default)
     Try gctpc if gctpc is enabled (default)
     try internal routines  

     note: older version of the priority was wrong.

   Note: some grids are only  supported by the internal routines

```




See also: 
[-geolocation](./geolocation.html)
















----

>Description: misc  X       X=0,1 use gctpc library (default=1)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/gctpc.html>_