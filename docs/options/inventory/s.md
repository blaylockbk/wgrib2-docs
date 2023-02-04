# wgrib2: -S

## Introduction

The -s option prints out a simple inventory with minutes and seconds.
-s is equivalent to
-T, -var, -lev, -ftime , and
-misc yyppp

```

-sh-2.05b$ wgrib2 new.grb2 -s
1:0:d=2007032600:HGT:1000 mb:anl:
2:125535:d=2007032600:HGT:1000 mb:3 hour fcst:

```

## Usage

```

-s

```

See also: [macros](./macros.html),
[-match_inv](./match_inv.html)

---

> Description: inv simple inventory with minutes and seconds (subject to change)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/S.html>_
