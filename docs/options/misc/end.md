# wgrib2: -end

## Introduction

The -end option stops the processing of the grib file after
one line of the inventory has been written. The -end option
is designed to improve speed when used with the -match option

## Usage

```
-end
```

### Example

```
$ wgrib2 ../example/eta.t00z.awphys18.grb2 -match "HGT:500 mb" -text hgt.txt -end

The above line writes out the first 500 mb HGT field to the file hgt.txt and stops
reading the file.

$ wgrib2 ../example/eta.t00z.awphys18.grb2 -match "HGT:500 mb" -text hgt.txt -end

The above line writes out all 500 mb HGT fields to the file hgt.txt.  This wastes
time if you only have one matching field.

```

See also: [-match](./match.md),
[-not](./not.md)

---

> Description: misc stop after first (sub)message (save time)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/end.html>_
