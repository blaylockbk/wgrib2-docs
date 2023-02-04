# wgrib2: -print

## Introduction

The -print option prints a string
into the inventory. It is used to make the inventory prettier.

## Usage

```
-print "string"
```

### Example

```
$ wgrib2 png.grb2 -print "VAR is" -var -print "LEV is" -lev
1:4:VAR is:RH:LEV is:2 m above ground
```

See also:
[-nl](./nl.md)

---

> Description: inv X inserts string (X) into inventory

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/print.html>_
