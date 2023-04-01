# -grib_out_irr

## Introduction

This option will be eliminated as template 3.130 was not added to the grib standard.

The grib2 format allows irregular grids; that is, grids that are defined by a list of latitude-longitude pairs.
This extension to the grib2 format allows you to handle models with unusual grids,
model data interpolated to a specific point and even observations.
Why would you use grib to handle point data rather than other
formats such as BUFR or netcdf? Some formats are difficult to use and converting between from grib to netcdf/BUFR
will lose metadata. Keeping the keeping the point data in grib facilitates the conversion between gridded
and collection-of-points data. For the conversion, wgrib2 can do nearest neighbor and a Cressman analysis.

### Space

Each grid point has a 4-byte latitude and a 4-byte longitude value. This is an
8 \* num_grid_point byte overhead. Since each grib message is independent, this
overhead has to be repeated for each grib message even if the grid is unchanged.
Howver, the grib format allows submessages so the overhead is only for the first
submessage. However, the length of any grib message has to be
less than 2 GB in order to be compatible a 32-bit machines. In practice you should
keep the message size smaller in order not to use up all the free memory.

## Usage

```
-irr_grid LON-LAT-LIST RADIUS OUT

LON-LAT-LIST   = lon-lat list, lon1:lat1:lon2:lat2:...:lon-n:lat-n
RADIUS         = radius in km
OUT            = output grib file

The -irr_grid option creates an irregular grid using nearest-neighbor interpolation.
If no input grid point is within RADIUS kms, the resulting grid has a undefined value.
```

### Example: make file with grid points at (10W,20N) (30W,40N)

```
$ wgrib2 flx.grb2 -irr\_grid 10:20:30:40 1000 2pt.grb2
1:0:d=2009010100:UFLX:surface:0-1 month ave fcst:
$ wgrib2 -V 2pt.grb2
1:0:vt=2009010100:surface:0-1 month ave fcst:UFLX Momentum Flux, U-Component [N/m^2]:
    ndata=2:undef=0:mean=0.0129:min=0.0035:max=0.0223
    grid_template=130:winds(N/S):
        Irregular Grid:(2 x 1) units 1e-06 input raw output raw res N/A
        lat=20.000000 40.000000
        lon=10.000000 30.000000

1st point lon=10W lat=20N
2nd point lon=30W lat=40N
```

See also:
[-grib_out_irr](grib_out_irr.md),
[-lola](lola.md),
[-cress_lola](cress_lola.md),

---

> Description: out X Y writes irregular grid grib (GDT=130 not adopted) X=(all|defined) Y=(output file)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/grib_out_irr.html>_
