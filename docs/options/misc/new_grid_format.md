# wgrib2: -new_grid_format

## Introduction

The option -new_grid_format specifies the output format.
for the -new_grid option.
The default is grib, with options for bin and ieee. The bin format is the
machine's native single precision floating point format. The ieee format is
single precision IEEE 754 format. The bin format can have optional f77-style
header. The ieee format be in big endian or little endian with an optional f77-style
header. The formats are the same as produced by
-bin and -ieee.
Headers are specified by
-header and -no_header.
The ieee format defaults to big endian (network order) and specified by
-little_endian and -big_endian.
The f77-style header is prepends and appends a 32-bit integer of the number
of bytes in the data field. The integer is appropriate to the endian of
the data and is stored in the native 32-bit integer format. Outputing in the
ieee format is slower than in the native bin format because there is a conversion
from the native format to ieee even if the native format happens to be ieee.

The output order of the fields may differ from the input order
of the fields. The problem occurs when -new_grid encounters
a vector interpolation. To do a vector interpolation, both the U and V must
be interpolated together. (Vector interpolation is needed so that the winds near the poles remain accurate.)
In order for the input order to be the same as the output order, the V field
must follow the corresponding U field. Wgrib2 allows a
sequence of "U-any number of scalar fields-V" to work with -new_grid
even though it is undocumented. To make sure the the input order is the same
as the output order, make sure V directly follows U, and also make sure you know
which fields are specified to be vector fields. (The default vectors are version specific.)

The -new_grid_format option was introduced with wgrib2 v3.0.0,
for support of the python interface. It requires the IPOLATES library to be set to ip2lib_d
rather than the older iplib library.

## Usage

```

-new_grid_format FORMAT
                 FORMAT = grib, the output format of -new_grid is grib2 (default)
                 FORMAT = bin, the output format is in native single precision format like -bin
                 FORMAT = ieee, the output format is in IEEE single precision like -ieee

```

### Example

```

    Creates a grib output file "new.grb"

$ wgrib2 gep19.aec -for 1:5 -new\_grid\_winds earth -new\_grid ncep grid 3 new.grb
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:70707:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
3:96843:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
4:125750:d=2009060500:UGRD:200 mb:180 hour fcst:ENS=+19
5:166391:d=2009060500:VGRD:200 mb:180 hour fcst:ENS=+19


    Creates a binary formated output file "new.bin", the scan order is we:ns
    U and V are paired, so the input order is the same as the output order

$ wgrib2 gep19.aec -for 1:5 -new\_grid\_format bin -new\_grid\_winds earth -new\_grid ncep grid 3 new.bin
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:70707:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
3:96843:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
4:125750:d=2009060500:UGRD:200 mb:180 hour fcst:ENS=+19
5:166391:d=2009060500:VGRD:200 mb:180 hour fcst:ENS=+19


   Comparing the binary and grib output files.

$ wgrib2 new.grb -rpn sto\_0 -import\_bin new.bin -rpn 'raw2:rcl\_0:print\_corr'
1:0:rpn_corr=1:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:130502:rpn_corr=1:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
3:203989:rpn_corr=1:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
4:261186:rpn_corr=1:d=2009060500:UGRD:200 mb:180 hour fcst:ENS=+19
5:350963:rpn_corr=1:d=2009060500:VGRD:200 mb:180 hour fcst:ENS=+19

   The binary file was written in we:ns, and -import_bin does not change
   the scan order.  Therefore we have to change the order we:sn by raw2.
   rpn_corr=1 .. new.grb == new.bin upto a grib rounding error

```

See also:
[-big_endian](./big_endian.md)
[-bin](./bin.md)
[-header](./header.md)
[-ieee](./ieee.md)
[-little_endian](./little_endian.md)
[-new_gridr](./new_grid.md)
[-no_header](./no_header.md)

---

> Description: misc X new_grid output format X=bin,ieee,grib

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/new_grid_format.html>_
