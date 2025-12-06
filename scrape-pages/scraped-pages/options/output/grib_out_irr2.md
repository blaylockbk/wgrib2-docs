# wgrib2: -grib_out_irr2

## Introduction

The option -grib_out_irr2 replaces
-grib_out_irr. The former uses Grid
Definition Template (GDT) 101 which is part of the grib
standard. The latter uses GDT 130 which was was not adopted.
The -grib_out_irr2 allows you to define
an unstructured grids which allows the locations of the grid
points to be arbitrary. For example, you can define an
unstructured grid to be the locations of the all the US
weather observing stations. One neat feature of GDT 101
is the grid number is a UUID (universally unique identifier)
which can be generated on the fly and is not "registered"
with the center or WMO. This feature, for example, will
allow you to generate a grid for all the ship observations
at 00Z January 1, 2017 and a different grid for any other
observation time. With the UUID feature of GDT 101, one
can store observational data in grib.

The locations of are not part of the metadata in grib message,
and the locations have to be provided another means such as
documentation at the center's web site or by including grib
mesages with the latitudes (NLAT) ane longitudes (ELON).

## Usage

```
-grib_out_irr2 NPNTS CENTER_GRID_NUBER REF_GRID_TYPE UUID OUTFILE

NPNTS              = number of grid points, can be differ from the size of the input grid
CENTER_GRID_NUMBER = use -1 unless your center has defined an appropriate grid number
REF_GRID_TYPE      = use -1 unless your center has defined an appropriate reference grid type
UUID               = universally unique identifier, use uuidgen to create a new UUID,
                     has a format: e1fc1f28-5024-4ff5-a04b-cf837d8574f6
                     0 = no UUID
OUTFILE            = output grib file
```

The option, -grib_out_irr2, can generate a grid with any number
of grid points. The data for the new grid is taken from the DATA register which is
usually the input data. If NPNTS is less than the size of the data register (NDATA),
then the first NPNTS of DATA are written out. If NPNTS is greater or equal to NDATA,
the DATA is written out and any extra points are set to undefined. For both cases,
there is no attempt to remap the data by finding the nearest grid point, etc. (At
this point, the latitudes and longitudes have not been specified.)

### Is GDT 101 Useful?

GDT 101 is useful! I can define a UUID, and make a grib files with 3 grib messages,
NLAT, ELON, TMP2m. Using wgrib2, I can interpolate TMP2m to a lat-lon grid.

I have a netcdf file with latitude(x,y), longitude(x,y) and TMP2m(t,latitude,longitude).
Using -import_netcdf, I can make grib message with ELON, NLAT and TMP2m. I can now
interpolate TMP2m to a lat-lon grid.

See also:

---

> Description: out 5 args writes irregular grid grib GDT 101 X=npnts Y=grid_no Z=grid_ref A=UUID B=(output file)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/grib_out_irr2.html>_
