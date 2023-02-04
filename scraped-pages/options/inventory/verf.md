### wgrib2 macros: -s -verf -V

## Introduction

The -s,
-verf
and -V
options are really macros
which are defined in the Macro.c file. The -s option is special because if there is no
"inv" option used, wgrib2 will add a -s option to the end of the argument list.

## Usage

```
-s
      equivalent to -t -var -lev -ftime -ens

-verf
      equivalent to -vt -var -lev -ftime -ens

-V
      equivalent to -vt -lev -ftime -var -ens -stats -grid

```

---

> Description: inv simple inventory using verification time

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/verf.html>_
