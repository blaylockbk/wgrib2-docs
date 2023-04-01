# -inv

## Introduction

The -inv option redirects the inventory to
a file. This option could be used with an option to send decoded
data to STDOUT.

```
$ wgrib2 file -inv my.inv -text - -no_header  -match ':HGT:500 mb:' |  JOB

  -inv my.inv   writes the inventory to my.inv
  -text -       write the decoded data as a text file to stdout
  -no_header    no header
  JOB           is some program that reads the raw data from stdin
```

The -inv option is similar but different from saving
stdout.

```
$ wgrib2 file -inv my.inv
$ wgrib2 file >my.inv2
```

In the above example, my.inv and my2.inv will be the same. However,
wgrib2 can be used as a grib filter. In this case, grib data is read
from stdin and written to stdout.

```
$ cat FILE1 FILE2 | wgrib2 - -match ':HGT:500 mb:' -grib - -inv /dev/null | \
   wgrib2 - -new_grid_winds earth -new_grid ncep grid 3 newgrd.grb
```

The -inv option is very specialized and helps
when you want to imbed wgrib2 within another program. For example,
reading a grib2 record from within perl program is easy.

```
  open(DATA, "wgrib2 file -inv /dev/null -text - -no_header -match ':HGT:500 mb:' |");
```

## Usage

```
-inv FILE
```

See also:
[-text](./text.md)
[-bin](./bin.bin)

---

> Description: init X write inventory to X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/inv.html>_
