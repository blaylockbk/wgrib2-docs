# wgrib2: -nl

## Introduction

The -nl option prints a new-line
into the inventory. It is used to make the inventory prettier.

## Usage

```
-nl
```

### Example

```
$ wgrib2 -gdt png.grb2 -s
1:4:d=2009060500:RH:2 m above ground:330 hour fcst:std dev
$ wgrib2 -gdt png.grb2 -s -stats
1:4:d=2009060500:RH:2 m above ground:330 hour fcst:std dev:ndata=65160:undef=0:mean=6.24625:min=0:max=29.3:cos_wt_mean=6.01318
$ wgrib2 -gdt png.grb2 -s -nl -stats
1:4:d=2009060500:RH:2 m above ground:330 hour fcst:std dev:
:ndata=65160:undef=0:mean=6.24625:min=0:max=29.3:cos_wt_mean=6.01318
```

See also:
[-print](./print.md)

---

> Description: inv> X write new line in file X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/nl_out.html>_
