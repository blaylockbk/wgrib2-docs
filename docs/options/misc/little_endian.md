###

wgrib2: -big_endian -little_endian

## Introduction

The -big_endian and the
-little_endian
options changes the order that IEEE numbers are read and written. This
does not affect the order in which binary numbers are read/written.

## Usage

```

-big_endian
   or
-little_endian

```

See also:
[-ieee](./ieee.md),
[-import_ieee](./import_ieee.md),
[-header](./header.md),
[-no_header](./header.md),

---

> Description: misc sets ieee output to little endian (default is big endian)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/little_endian.html>_
