# -get_byte

## Introduction

The -get_byte option prints the values of
selected bytes in the grib message. For example if you want to
see the 20 and 21 octet (byte in WMO speak) of section 4, you would
use -get_byte 4 20 2. The first argument
is the section number. The second is the byte (octet) number starting
from 1 (consistent with WMO documentation) and the third is the number
of octets to display.

## Usage

```
-get_byte SECTION OCTET NUMBER
SECTION = section of the grib message to print
OCTET = starting octet to start printing, OCTET ≥ 1
NUMBER = number of bytes to print
```

### Example

```
$ wgrib2 f.grb2 -get\_byte 0 1 16
1:0:0-1=71,82,73,66,0,0,0,2,0,0,0,0,0,0,18,178
```

See also:
[new grib](new_grib.md),
[-set_byte](set_byte.md),
[-get_ieee](get_ieee.md),
[-get_int](get_int.md),
[-get_hex](get_hex.md)

---

> Description: inv X Y Z get bytes in Section X, Octet Y, number of bytes Z (decimal format)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/get_byte.html>_
