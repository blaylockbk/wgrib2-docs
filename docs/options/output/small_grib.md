# -small_grib

## Introduction

The -small_grib option writes the grid values
to a grib2 file with the same grid spacing but a smaller domain.
The grid point locations are unchanged. This option is used
to make a regional subset and only works for certain grids
such as the lat-lon, rotated lat-lon, Mercator, Lambert conformal, and polar stereographic.

When -small_grib option has problems, the grid is not subsetted
and the original grid is written. Some reasons for problems are

1. Unsupported grid type

- Thinned grid
- The bounding box is outside of the grid domain
- The bounding box is too small and does not include a grid point
- The bounding box definition is not right (ex. LatN < LatS)

The -ijsmall_grib option is similar to the
-small_grib option except it uses the grid coordinates. The former is
faster as it doesn't have to compute the latitudes and longitudes of the grid points and find
a bounding box.

There are other ways of making a grib file which only includes the data for a
subregion. You can set the points outside of your region of interest to UNDEFINED
using the -undefine or -ijundefine options.
Once the grid points are set to UNDEFINED, many of the packing methods will reduce the
size of the new grib file. (Complex packing without bitmaps is very good.) One can also interpolate
the field to a new grid using the -new_grid option.

## Usage

```
-small_grib LonW:LonE LatS:LatN file_name
```

For west longitudes and south latitudes, you can use negative values.
The file_name is the output file. LonE must have a numerical
value greater than LonW. For example for left boundary=20W and
the right boundary=60E, you can use LonE=340 and LonW=420.
You can also use LonE=-20 and LonW=60.

### Example

```
$ wgrib2 fcst.grb2 -small\_grib 10:20 -20:20 small.grb
1:0:d=2007032600:HGT:1000 mb:anl:

$ wgrib2 small.grb -grid
1:0:grid_template=0:
        lat-lon grid:(21 x 81) units 1e-06 input WE:SN output WE:SN res 48
        lat -20.000000 to 20.000000 by 0.500000
        lon 10.000000 to 20.000000 by 0.500000 #points=1701
```

The first line writes a small grib file from 10E-20E and 20S-20N. Often
you want to preserve the grib compression. In this case, you would
add the option -set_grib_type same to the wgrib2
command line.

See also:
[-ijsmall_grib](./ijsmall_grib.md)
[-ijundefine](./ijundefine.md),
[-new_grid](./new_grid.md)
[-set_grib_type](./set_grib_type.md)
[-undefine](./undefine.md),

---

> Description: out X Y Z make small domain grib file X=lonW:lonE Y=latS:latN Z=file

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/small_grib.html>_
