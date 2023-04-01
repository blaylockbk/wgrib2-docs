# -set_ieee

## Introduction

The -set_ieee option sets 4 octets to a
single precision IEEE floating point value. The IEEE floating point standard
is often used grib2.

```
-set_ieee I J X
  I = Section = 1..7
  J = 1..(section length-3)
  X = single precision floating point number
```

Multiple floats can be set by making the third argument a colon seperated list.

## Usage

```
-set_int  SECTION STARTING_OCTET_LOCATION I-1:I-2:..:I-N
SECTION=0 .. 7
OCTET_LOCATION = 1..N
I-M = Mth floating point number
```

### Example

See also:
[-get_int](get_int.md)
[-set_byte](set_byte.md)
[-set_hex](set_hex.md)
[-set_int2](set_int2.md)

---

> Description: misc X Y Z set ieee float in Section X, Octet Y, floats Z (a|a:b:c)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_ieee.html>_
