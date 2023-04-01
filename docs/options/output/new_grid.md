# -new_grid

![New!](../icons/new.png)

Recently discovered bug in IPOLATES. To avoid the bug, all grids should have a size of greater
than 1 degree longitude. For example, delta_longitude = 0.1 degree, then number of longitude points
needs to be ≥ 11 to make grid > 1 degree.
The IPOLATES library wanted to handle the case of a global grid going from
xE to (x+360)E which is be encoded as going from xE to xE. To allow for rounding errors,
a grid from xE to yE (y-x ≤ 1) was condsidered to be a grid from xE to (360+x)E.
This causes a problem when the grids are ≤ than 1 degree in longitude. This bug will occur
occur for both the input and output grids.

## Introduction

Beginners are encouraged to read the [new_grid introduction](./new_grid_intro.md).

The -new_grid option interpolates the fields to a new
grid.
The default interpolation is bilinear but that can be changed using the
-new_grid_interpolation option. This option uses
scalar and vector interpolation as appropriate. In order for the vector
interpolation to work, the vector quantities must be in a (U,V) order.
For example: Z200, U200, V200, Z500, U500, V500 is good. If the data
are not in (U,V) order, the inventory can be sorted into (U,V) order
and the inventory can then be used to control the order of processing.
If the vector quanties are not in (U,V) order, the vector quantites will
not be interpolated. (See example 3 for converting the file into a (U,V) order.)

The option is not part of the default configuration but it included with many distributions.
The interpolation code is written
in fortran and combining fortran and C code can require some work. (gcc/gfortran and
clang/gfortran are already handled by the makefile.) Getting the C and Fortran code to cooperate
requires some system-specific knowledge and may not be possible in all cases. Consequently
you are on your own in getting the -new_grid option installed.

Operations often has to use the -new_grid option to produce a large number
of user grids. Fortunately the interpolation can be made embarrassingly parallel. A
portable single-node solution is described in here. A
multi-node solution is possible using MPI and the wgrib2 library.

### Caution

The -new_grid option works in raw scan mode, so data are not
converted to sn:we order. Conequently options that only work in sn:we order cannot
work at the same time as the -new_grid option. Any option
that uses geolocation (ex. -rpn, -lon)
is incompatible with -new_grid.

### Winds

Before you do an interpolation, you need to define the wind directions.
Most people want the the V winds to be in the direction of the North Pole.
With a verbose wgrib2 inventory, you will seee winds(N/S). However, some
meteorologists want the V winds to go from grid point (i,j) to (i,j+1).
The corresponding wgrib2 notation is "winds(grid)". See the
[-new_grid_winds](./new_grid_winds.md) option for more details.

## Usage

```
-new_grid_winds W -new_grid A B C outfile
    W = earth or grid
        earth means that the U wind goes eastward, V goes northward
        grid means that U wind goes from grid (i,j) to (i+1,j)
          which is not eastward in a Lambert-conformal or polar stereographic grids
    A, B, C are the output grid description
    outfile is an output file.  The grib2 interpolated records are written in outfile
```

## Grid description format

The nx and ny parameters are integers such that nx\*ny < 2147483648.
Angles and delta-angles are in degrees, and are rounded to micro-degrees unless
otherwise specified. Dx and Dy are in meters and are usually rounded
to the nearest mm.

The interpolation library does not handle non-spherical grids. The library
also has problems with grid smaller than 1 degree in longitudinal width. It assumes
that the calculations are not precise, and the grid is global.

```
### General Format


-new_grid A B C outfile                                                   General format

    A                                                                     grid name with parameters
    B                                                                     X or longitude description (ex lon0:nlon:dlon)
    C                                                                     Y or latitude description (ex lat0:nlat:dlat)

### Selected NCEP Grids (not in general format)


-new_grid ncep grid I outfile                                   	  I = 2,3,4,45,98,126-129,170,173,184,194,221,230,242,249
                                                                            (NCEP grid defintions)
									  T62,T126,T170,T190,T254,T382,T574,T1148,T1534
									    (NCEP Gaussian grids definitions)
                                                                          Want more ncep grids?  Modify ncep_grids.c


### Latitude-Longitude Grid


-new_grid latlon lon0:nlon:dlon lat0:nlat:dlat outfile		  	  lat-lon grid
									  lat0, lon0 = degrees of lat/lon for 1st grid point
									  nlon = number of longitudes
									  nlat = number of latitudes
									  dlon = grid cell size in degrees of longitude
									  dlat = grid cell size in degrees of latitude


### Rotated Latitude-Longitude Grid (requires ip2lib\_d)


-new_grid rot-ll:sp_lon:sp_lat:sp_rot lon0:nlon:dlon lat0:nlat:dlat outfile
                                                                          rotated latitude-longitude grid
                                                                          sp_lon = longitude of the South pole (for rotation)
                                                                          sp_lat = latitude of the South pole (for rotation)
                                                                          sp_rot = angle of rotation (degrees)
                                                                           The grid is defined in the rotated coordinates.
									  eat0, lon0 = degrees of lat/lon for 1st grid point
									  nlat = number of longitudes
									  nlon = number of latitudes
									  dlon = grid cell size in degrees of longitude
									  dlat = grid cell size in degrees of latitude


### Lambert Conic Grid


-new_grid lambert:lov:latin1:latin2:lad lon0:nx:dx lat0:ny:dy outfile	  Lambert conic conformal
-new_grid lambert:lov:latin1:latin2 lon0:nx:dx lat0:ny:dy outfile         lad = latin2
-new_grid lambert:lov:latin1 lon0:nx:dx lat0:ny:dy outfile                latin2 = latini lad = latin1
									  lov = longitude (degrees) where y axis is parallel to meridian
									  latin1 = first latitude from pole which cuts the secant cone
									  latin2 = second latitude from pole which cuts the secant cone
									  lad = latitude (degrees) where dx and dy are specified
									  lat0, lon0 = degrees of lat/lon for 1st grid point
									  nx = number of grid points in X direction
									  ny = number of grid points in Y direction
									  dx = grid cell size in meters in x direction
									  dy = grid cell size in meters in y direction
                                                                          note: if latin2 >= 0, the north pole is on proj plane
                                                                                if latin2 < 0, the south pole is on proj plane


### Lambert Conic Grid with Centered Location


-new_grid lambertc:lov:latin1:latin2:lad lonc:nx:dx latc:ny:dy outfile    Lambert conic conformal with centered position
-new_grid lambertc:lov:latin1:latin2 lonc:nx:dx latc:ny:dy outfile        like lambert except lonc and latc replace lon0 and lat0
-new_grid lambertc:lov:latin1 lonc:nx:dx latc:ny:dy outfile               latc, lonc = degrees of lat/lon for center of the grid


### Polar Stereographic Grid


-new_grid nps:lov:lad lon0:nx:dx lat0:ny:dy outfile                       north polar stereographic
-new_grid sps:lov:lad lon0:nx:dx lat0:ny:dy outfile			  south polar stereographic
									  lov = longitude (degrees) where y axis is parallel
                                                                            to meridian
									  lad = latitude (degrees) where dx and dy are specified
                                                                            note: grib1 uses lad = 60N (nps) or 60S (sps)
                                                                            lad must be 60 (nps) or -60 (sps) (library limitation)
									  lat0, lon0 = degrees of lat/lon for 1st grid point
									  nx = number of grid points in X direction
									  ny = number of grid points in Y direction
									  dx = grid cell distance meters in x direction at lad
									  dy = grid cell distance meters in y direction at lad


### Global Gaussian Grid


-new_grid gaussian lon0:nx:dlon lat0:ny outfile                           global Gaussian grid
                                                                          lat0, lon0 = degrees of lat/lon for 1st grid point
                                                                          note: lon1 = -lon0, lat1 = lat0 + (nx-1)*dlon;
									  nx = number of grid points in X direction
									  ny = number of grid points in Y direction
                                                                             ny must be even
                                                                          dlon = degrees of longitude between adjacent grid points

                                                                          note: wgrib2 supports regional Gaussian grids, but
                                                                          the interpolation library doesn't.

### Mercator Grid


-new_grid mercator:lad lon0:nx:dx:lonn lat0:ny:dy:latn outfile            mercator grid
                                                                          lad = latitude (degrees) where dx and dy are specified
                                                                          lat0, lon0 = degrees of lat/lon for 1st grid point
                                                                          latn, lonn = degrees of lat/lon for last grid point
                                                                          nx = number of grid points in X direction
                                                                          ny = number of grid points in Y direction
                                                                          dx = grid cell distance in meters in x direction at lad (double precision)
                                                                          dy = grid cell distance in meters in y direction at lad (double precision)

                                                                          see [grib2 template](https://www.nco.ncep.noaa.gov/pmb/docs/grib2/grib2_doc/grib2_temp3-10.shtml) for the Mercator grid

                                                                          note: dx and dy are in meters unlike the grib template which is in mm (integer).
                                                                          note: dx and dy depend on the radius of the earth as specified by the grib message
                                                                          note: interpolation library assumes that earth will be spherical
                                                                          note: the mercator grid description is over specified
                                                                          User must make sure (nx,dy) is consistent with lonn
                                                                          as well as (ny,dy) is consistent with latn

lambert, nps, sps, mercator only support we:sn ordering
latlon, gaussian only support we:sn and we:ns ordering

### Grib (wgrib 3.0.0, ip2lib\_d)


-new_grid grib file Y                                                     grid definition from a grib file
                                                                          file=grib2 file
                                                                          Y (ignored)

                                                                          note: this option is only available on wgrib2 v3.0.0+
                                                                              using ip2lib_d (IPOLATES=3)
                                                                          note: default wind rotation is obtained from file
                                                                          note: support for grib files is limited by ip2lib_d

### Location (wgrib 3.0.0, ip2lib\_d)


-new_grid location list UUID                                              unstructured grid
                                                                          list = lon1:lat1:lon2:lat2:...:lon-n:lat-n
                                                                          UUID = UUID for this grid, 0 if none

                                                                          note: wind rotation is set to earth
                                                                          The locations are saved to the grib file as the first
                                                                          and second grib messages. (3 decimal digits precision)

```

Sometimes one wants to convert grib files to a common grid. You already
have a sample grib file and you have to figure out the grid format.
For common grids, you can use the perl script
[grid_defn.pl](https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2.scripts/grid_defn.pl)
to determine the grid format.

```
$ grid_defn.pl small.grb2
latlon 0.000000:2:10.000000 20.000000:2:8$

grid_defn.pl does not add a newline at the end of its output.
Not all grids are supported.
```

### Examples

```
  wgrib2 IN.grb -set_grib_type same -new_grid_winds earth -new_grid latlon 100:10:1 30:20:1 OUT.grb
    Interpolates from IN.grb to OUT.grb
    Uses the same grib packing as in the input file
    Makes a 10x20, 1x1 degree lat-lon grid, lower left corner: 100E 30N


  wgrib2 IN.grb -set_grib_type same -new_grid_winds earth -new_grid ncep grid 221 OUT.grb
    Interpolates from IN.grb to OUT.grb
    Uses the same grib packing as in the input file
    Interpolates to NCEP grid 221.

  wgrib2 IN.grb -set_grib_type same -new_grid_winds earth -new_grid `grid_defn.pl OLD.grb` OUT.grb
    Interpolates from IN.grb to OUT.grb
    Uses the same grib packing as in the input file
    Interpolates using the grid format of OLD.grb (1st record)

 In this example, U and V are not in the required order.  This
shows a sorting to the required order for -new_grid to work.

    wgrib2 201201.A | sed -e 's/:UGRD:/:UGRDa:/' -e 's/:VGRD:/:UGRDb:/'  | \
    sort -t: -k3,3 -k5,8 -k4,4 | \
    wgrib2 201201.A -i -new_grid_winds earth -new_grid ncep grid 2 201201.A.grd2

    The first line creates an inventory with new variable names: UGRD -> UGRDa and VGRD -> UGRDb
    The second line sorts the inventory so that UGRDb follows UGRDB.
    The third line regrids the file, with the order of processing controlled by the inventory.
```

### Type of Interpolation

The IPOLATES library (either iplib or ip2lib_d) supports a number of interpolation schemes including bilinear (default),
bicubic, neighbor and budget. (In addition, ip2lib_d will support spectral.)
The interpolation method can be selected by using the -new_grid_interpolation
option before the -new_grid option. Some of the interpolation
options need numeric parameters which are set by the
-new_grid_ipopt option. IPOPT is defined in the iplib library documentation.

You can use different interpolations for different variables. For example, a
bilinear interpolation of soil or vegetation type is meaningless. So
nearest neighbor interpolation is used instead.

```
    wgrib2 IN.grb -new_grid_winds earth \
      -new_grid_interpolation bilinear \
      -if ":(VGTYP|SOTYP):" -new_grid_interpolation neighbor -fi \
      -new_grid latlon 0:360:1 90:181:-1 OUT.grb

   line 2: set default interpolation to bilinear
   line 3: if VGTYP or SOTYP then set the interpolation to nearest neighbor
   line 4: do the interpolation
```

Comments (4/2018): When you convert from a high resolution grid
to a lower resolution grid, you have to be consider changing from
the default interpolation (bilinear) to a budget interpolation.
The budget gives a better estimate of the cell average (the default
is 25 bilinear interpolations).

### Changing from grid-relative to Earth-relative winds and vice versa

Most NCEP grib files use grid-relative winds. If you want to convert to Earth-relative winds
or grid-relative winds, you can use the -new_grid option.

```
   To Earth relative:
     wgrib2 IN.grb -set_grib_type same -new_grid_winds earth -new_grid_interpolation neighbor \
      -new_grid `grid_defn.pl IN.grb` OUT.grb

   To Grid relative:
     wgrib2 IN.grb -set_grib_type same -new_grid_winds grid -new_grid_interpolation neighbor \
      -new_grid `grid_defn.pl IN.grb` OUT.grb

"-set_grib_type same" preserves the grib packing or compression
"-new_grid_interpolation neighbor" should be faster than the default bilinear
`grid_defn.pl IN.grb` returns the grid definition of the first grib message in IN.grb
```

The limitations of the above command are

- IN.grb can only have one grid type
- OUT.grb will have any submessages converted into messages

### Changes from copygb

People may want to convert from copygb and copygb2 to wgrib2's -new_grid. Some differences
to keep in mind.

1. copygb default vectors: UGRD/VGRD

- wgrib2 default vectors: depends on version of wgrib2. See [new_grid_vectors](./new_grid_vectors.md).
- copygb can have vectors in any order
- wgrib2 must have V follow U for vectors pairs
- copygb has bilinear, bicubic, nearest neighbor, budget, neighbor budget, and spectral interpolations.
- wgrib2 has bilinear, bicubic, nearest neighbor, and budget interpolations. (Spectral is in development.)
- wgrib2 can select the interpolation type depending on the variable (ex soil type)
- copygb uses fixed Earth's radius
- wgrib2 uses Earth's radius based on grib message
- wgrib2 doesn't have merging, mapthreshold or map files
- copygb by default, ignores the binary scaling and preserves decimal scaling
- wgrib2 by default, preserves binary and decimal scaling
- copygb does grib1.
- copygb2 does grib2.
- wgrib2 does grib2.

### Speed: Interpolation Weights

The first step of the -new_grid interpolation is
to calculate the interpolation weights. (Each grid point on the new grid
is a weighted average of a small set of the old grid points.)
To save time for future interpolations, the last set of weights is saved.
Consequently interpolation is fastest when the input and output grids remain
don't change. While one can have multiple -new_grid options on
the command line, it is not recommended because the caching of the weights
wouldn't work and weights would have to be recalculated every time.

### Installation

```
With wgrib2 v2.0.8+, you have an option of compiling with the old iplib or
the new ip2lib_d library.  With the older versions of wgrib2, you are limited
to the old iplib or an hwrf version of that library.  It is recommended that
you use the newer ip2lib_d because it support latitude and longitudes to
micro-degrees and the WMO definition of rotated lat-lon grids.  However,
the new library doesn't suport the NCEP local grids such as 32769 as used
by the NAM native files.

 With wgrib2 v2.0.8+, you set USE\_IPOLATES to 3 in the makefile
for ip2lib\_d. To use the old iplib, you set USE\_IPOLATES to 1. If you
don't want the -new\_grid options, you can set USE\_IPOLATES to 0.


```

### Converting from WE:SN to WE:NS Grids

Many of wgrib2 grib2 writing options will write the grid
in WE:SN order. This natural because geolocation is
only enabled when the internal grids are in WE:SN order.
However, some codes need the grid in WE:NS order. To
convert a grib file from WE:SN order to WE:NS order, the
simplest way is to use -new_grid. Lat0 and lon0 need
to be lat/lon of the top left corner of the grid. Dlon
will a positive number and dlat will be negative.

If you want to be tricky, you can do a variation of the
"NDFD work arounds" technique. It will be faster and
more generic.

### Thinned Gaussian Grids to other Grids

Converting from a thinned Gaussian grid is a two step process
using wgrib2. First you convert from the thinned grid to full
grid using [-reduced_gaussian_grid](./reduced_gaussian_grid.md).
Then you can use -new_grid to interpolate to your desired grid.

### Quilting tiles - Merging files

Yes, it has been done using -new_grid, -import_grib -rpn/merge, and
the -grib_out options. Yes, at least 3 people have done it. The best
application combined a global oceanic and various regional oceanic forecasts.
Priority was given to the model with the highest resolution. This is convenient
for the user who is on a route that covers different model domains.

### Limitations of the iplib library

The iplib library has its limitations.
Grids types used by NCEP get supported and others don't. The library has a grib1 interface, so
new features of grib2 are not supported and the precision of the grid parameters
are limited to the grib1 values. For example, latitude and longitude
values are limited to millidegrees instead of microdegrees. Note: during
the installation of ip2, I noticed that the longitude was reduced when
reducing the range to [0,360). (longitude = amod(longitude + 3600.0, 360.0))
For single precision numbers, this reduces the precision to about two digits after
the decimal place.

1. not all grid types are supported by iplib

- only common grids are supported by the wgrib2 "wrapper" for iplib
- latitude, longitude values are nomially in millidegrees (affects interpolation)
- Single precision reals are used. The effective precision of the
  longitudes is to a hunderdth of a degree.
- only grib1 scan order are supported (i.e., WE:SN and WE:NS)
- NDFD/Glahn scan order is not supported (i.e., WE|EW:SN)
- Earth is assumed to be spherical
- Lambert conformal: LatD must follow grib1 conventions
- Polar Stereographic: LatD must be 60 latitude (grib1 convention)
- nx, ny, npts must be ≤ 2147483647. (Grib2 standard is 4294967295.)
- Spectral interpolation is not supported.
- grids should be at least 1 degree by 1 degree.

### Limitations of the ip2lib_d library

As of wgrib2 v2.0.8, ip2lib_d is the default interpolation library.
Ip2lib_d is the double-precision grib2-interface IPOLATES library. The
old ip2lib will remain a compile time option. The advantages of
ip2lib_d that it supports grib2 precision (one millionth of a degree),
at rotated lat-lon grids (Grid Definition Template 3.1).

The speed of the single precision iplib was originally faster than
the double precision ip2lib_d. To remove this objection from converting
to the new library, a effort was made to increase the OpenMP threading.
Consequently the run time of ip2lib_d will be similar (bilinear) to much
faster (budget) than iplib in a multiprocessor environment.

Wgrib2 v3.0.0 adds an optional spectral interpolation.

1. supports more grids than iplib including WMO defined rotated lat-lon.

- latitude, longitude values are in microdegrees (affects interpolation)
- Double precision reals are used. (Wgrib2 stores is grid point values
  as single precision.)
- grib1 polar stereographic projection, LaD = 60N or 60S has been removed
- NDFD/Glahn scan order (i.e., WE|EW:SN) may be supported in the future.
- staggering will be supported unlike iplib
- Some interpolations support an ellipical earth.
- Lambert conformal: LatD must follow grib1 conventions
- nx, ny, npts must be ≤ 2147483647. (Grib2 standard is 4294967295.)
  This is a limitation of compiling using 4 byte integers.
- Does not support local NCEP grid definitions such as 32769
- Spectral interpolation is limited to global fields with no undefined values.
- grids should be at least 1 degree by 1 degree.

### NDFD work arounds

NDFD files are often written (WE|EW):SN order. This means that the odd rows
are in WE order and the even rows are EW order. The rows go from south to north.
If you are using wgrib2 that is using iplib, the simplest solution
to convert the grid to WE:SN order is

Version 0 2. for data in input=WE|EW:SN scan order (wgrib2 IN -scan)

- read data, change order scan order of data, change flag table 3.4, save data
- wgrib2 IN.grb -rpn alt_x_scan -set table_3.4 64 -grib_out OUT.grb

Version 1 2. Read trick 55 in [wgrib2 tricks](https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/tricks.wgrib2)

Version 1

A variation of the previous trick can be used to put data in to (WE|EW):SN packing.

See also:
[-new_grid_format](./new_grid_format.md),
[new_grid introduction](./new_grid_intro.md),
[-new_grid_interpolation](./new_grid_interpolation.md),
[new_grid order](./new_grid_order.md),
[-new_grid_winds](./new_grid_winds.md),
[-new_grid_vectors](./new_grid_vectors.md),
[-lola](./lola.md),
[-bin](./bin.md),
[-import_bin](./import_bin.md),
[-rpn](./rpn.md),
[-grib_out](./grib_out.md),
[new_grid multi-core usage](./new_grid_usage.md),

---

> Description: out X..Z,A bilinear interpolate: X=projection Y=x0:nx:dx Z=y0:ny:dy A=grib_file alpha

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/new_grid.html>_
