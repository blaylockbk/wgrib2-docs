# -get_int

## Introduction

The -get_int option prints the values of
selected four-byte integers in a grib message. For example if you want to
see the number of data points (section 3, octets 7-10), you could
use -get_int 3 7 1. The first argument
is the section number. The second is the octet number starting
from 1 (consistent with WMO grib documentation) and the third is the number
of integers to display. The -get_int option uses
the grib format for signed 4-octet integers.

## Usage

```
-set_int SECTION OCTET_NUMBER COUNT
```

### Example

```
$ wgrib2 f.grb -get\_int 3 7 1
1:0:3-7=65160
```

See also:
[new grib](new_grib.md),
[-get_byte](get_byte.md)
[-get_int2](get_int2.md)
[-get_hex](get_hex.md)

---

> Description: inv X Y Z get 4-byte ints in Section X, Octet Y, number of ints Z

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/get_int.html>_
