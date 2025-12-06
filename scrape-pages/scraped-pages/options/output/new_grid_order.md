# wgrib2: -new_grid_order

## Introduction

The option -new_grid requires that the grib
file be processed in a certain order. When vector fields are encountered,
the specifications are that the U field must be followed by the corresponding V field.
(The -new_grid implementation allows any number of scalars to in between the U and
corresponding V fields.) If you do not follow the specification, some U or V
fields may not be interpolated.

Regridding grib files by the -new_grid is very common, and
a technique have been developed to speed up this task. Basically you put scalar
fields in its own grib message, and corresponding vector fields in their own grib
message (U and V are in submessages). Then you can regrid each grib message independantly.
If you have N cores, you run N copies of wgrib2 that regrids 1/N of the file.
The -new_grid_order is designed to put the data in this structure.

The option -new_grid_order rearranges the file so that
the fields follow the specification for use by -new_grid_order.
Note that the order depends on the fields that are specified as vectors by
the -new_grid_vector option.
The output of -new_grid_order puts the U and corresponding V
grib message into the same grib message, like -submsg_uvr.
The vector fields that cannot be pair with the corrsponding U or V fields are written
to a secondary file.

## Usage

```
-new_grid_order GRIB_A GRIB_B
                GRIB_A is a grib output file with data in order compatible with -new_grid
                GRIB_B is a grib output file with data that cannot be processed by -new_grid
                   because corresponding U or V fields were missing
```

### Example

```
$ wgrib2 gep19.badorder -new\_grid\_winds earth -new\_grid ncep grid 3 test.grb
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:70707:d=2009060500:UGRD:200 mb:180 hour fcst:ENS=+19
3:111348:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
-new_grid: VGRD doesn't pair with previous vector field, field ignored
4:137484:d=2009060500:VGRD:250 mb:180 hour fcst:ENS=+19
5:182284:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
6:211191:d=2009060500:VGRD:200 mb:180 hour fcst:ENS=+19
7:254630:d=2009060500:HGT:250 mb:180 hour fcst:ENS=+19
8:325988:d=2009060500:TMP:250 mb:180 hour fcst:ENS=+19
9:351022:d=2009060500:UGRD:250 mb:180 hour fcst:ENS=+19
10:393024:d=2009060500:RH:250 mb:180 hour fcst:ENS=+19
11:424624:d=2009060500:HGT:500 mb:180 hour fcst:ENS=+19
-new_grid: last field UGRD was not interpolated (missing V)

(failed, U and V are not in the proper order)

$ wgrib2 gep19.badorder -new\_grid\_order - junk | \
 wgrib2 - -new\_grid\_winds earth -new\_grid ncep grid 3 test.grb
1:3:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:70820:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
3:97067:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
4.1:126028:d=2009060500:UGRD:200 mb:180 hour fcst:ENS=+19
4.2:126028:d=2009060500:VGRD:200 mb:180 hour fcst:ENS=+19
5:210051:d=2009060500:HGT:250 mb:180 hour fcst:ENS=+19
6:281464:d=2009060500:TMP:250 mb:180 hour fcst:ENS=+19
7.1:306553:d=2009060500:UGRD:250 mb:180 hour fcst:ENS=+19
7.2:306553:d=2009060500:VGRD:250 mb:180 hour fcst:ENS=+19
8:393299:d=2009060500:RH:250 mb:180 hour fcst:ENS=+19
9:424954:d=2009060500:HGT:500 mb:180 hour fcst:ENS=+19

(worked)
```

See also:
[-new_grid](./new_grid.md)
[-new_grid_vectors](./new_grid_vectors.md)
[-submsg_uv](./submsg_uv.md)

---

> Description: out X Y put in required order for -new_grid, X=out Y=out2 no matching vector

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/new_grid_order.html>_
