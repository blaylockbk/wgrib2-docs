# wgrib2: -t -T -vt -VT

## Introduction

The -t -T -vt -VT options prints various time flags.
The -t -T prints the reference time
and -vt -VT prints the verification time.
The capitalized versions print the time with the seconds
and the lower case options print the time with out the seconds.
In conjuntion with -v2 verbose mode, the
format of the time will change to be GrADS compatible.

## Usage

```

-t
-T
-vt
-VT

```

### Example

```

$ wgrib2 g720\_360.grb2 -t
1:4:d=2009010100
$ wgrib2 g720\_360.grb2 -t -v2
1:4:00Z01jan2009
$ wgrib2 g720\_360.grb2 -T
1:4:D=20090101000000
$ wgrib2 g720\_360.grb2 -vt
1:4:vt=2009010600
$ wgrib2 g720\_360.grb2 -vt -v2
1:4:00Z06jan2009
$ wgrib2 g720\_360.grb2 -VT
1:4:vt=20090106000000

```

See also:

---

> Description: inv reference time YYYYMMDDHHMMSS

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/T.html>_
