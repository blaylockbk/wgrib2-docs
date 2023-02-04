# pywgrib2_s: mk_inv

## Introduction

The routine pywgrib2_s.mk_inv(grib_file, inv_file) reads a grib_file and writes
the inventory to inv_file. Both the grib_file and inv_file can be memory files.
The inventory will change with the version of wgrib2 by the addition of new fields.

### Example 1

```
>>> pywgrib2_s.mk_inv('a.grb','a.inv')
0
>>> a=pywgrib2_s.read_inv('a.inv')
>>> print(a[0])
1:0:D=20200101000000:CDCON:high cloud layer:0-6 hour ave fcst::CDCON:n=1: ..
>>> print(a[1])
2:2808:D=20200101000000:CDCON:middle cloud layer:0-6 hour ave fcst::CDCON: ..
```

### Example 2

```
>>> pywgrib2_s.mk_inv('a.grb','a.inv',Short=True)
0
>>> a=pywgrib2_s.read_inv('a.inv')
>>> a[0]
'1:0:D=20200101000000:CDCON:high cloud layer:0-6 hour ave fcst:'
>>> a[1]
'2:2808:D=20200101000000:CDCON:middle cloud layer:0-6 hour ave fcst:'
```

## Usage

```
     err=pywgrib2_s.mk_inv(grib_file, inv_file [, optional arguments])
         grib_file = name of a grib file
         inv_file = name of the inventory file to be created
         err = 0 if no error
               non-zero for error

     Optional arguments:
         Short =          False (default) make standard long inventory, wgrib2 -Match_inv
                          True make short inventory, wgrib2 -S
         Use_ncep_table = False (default) grib table as defined by center in the grib file
                          True, use NCEP grib tables, for centers that adopt NCEP grib tables
```

## Usage

[overview](./pywgrib2_s.md)
[next](./pywgrib2_s_read_inv.md)

---

> Description: Makes inventory file

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pywgrib2_s_mk_inv.html>_
