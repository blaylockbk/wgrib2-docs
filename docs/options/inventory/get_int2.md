# wgrib2: -get_int2

## Introduction

The -get_int2 option prints the values of
selected two-byte integers in a grib message. For example if
you wanted to see octet N and N+1 is section M as a signed
two byte integer, you could
use -get_int2 M N 1. The first argument
is the section number. The second is the octet number starting
from 1 (consistent with WMO grib documentation) and the third is the number
of integers to display. The -get_int2 option uses
the grib format for signed 2-octet integers.

## Usage

```

-set_int2 SECTION OCTET_NUMBER COUNT

```

### Example

```

$ wgrib2 f.grb -get\_int2 1 6 1
1:0:1-6=7

```

See also:
[new grib](new_grib.md),
[-get_byte](get_byte.md)
[-get_int](get_int.md)
[-get_hex](get_hex.md)

---

> Description: inv X Y Z get 2-byte ints in Section X, Octet Y, number of ints Z

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/get_int2.html>_
