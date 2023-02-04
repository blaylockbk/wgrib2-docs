# wgrib2: -ijsmall_grib

## Introduction

The -ijsmall_grib option writes the grid values
to a grib2 file with the same grid spacing but a smaller domain.
It is similar to the -small_grib option
except it uses i,j values rather than lat-lon values. The grid point
locations are unchanged. This option is used to make a regional subset
and only works for certain grids such as the lat-lon, rotated lat-lon, Mercator and Lambert conformal.

## Usage

```

-ijsmall_grib ix0:ix1 iy0:iy1 file_name

Where, 1 <= ix0 < ix1 < nx
Where, 1 <= iy0 < iy1 < ny
By default, (i,j) is the South-West corner

File_name is the output grib2 file

```

### Example

```

$ wgrib2 fcst.grb2 -ijsmall\_grib 10:20 20:30 small.grb
1:0:d=2007032600:HGT:1000 mb:anl:

$ wgrib2 small.grb -grid
1:0:grid_template=0:
        lat-lon grid:(11 x 11) units 1e-06 input WE:SN output WE:SN res 48
        lat -80.500000 to -75.500000 by 0.500000
        lon 4.500000 to 9.500000 by 0.500000 #points=121

```

See also: [-undefine](./undefine.md),
[-small_grib](./small_grib.md)

---

> Description: out X Y Z make small domain grib file X=ix0:ix1 Y=iy0:iy1 Z=file

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ijsmall_grib.html>_
