# wgrib2: -0xSec

## Introduction:

The -0xSec option prints a hex dump of any of
the grib sections. The format of the hex dump depends on the
verbosity level.

```
$ wgrib2 png.grb2 -0xSec 0
1:4:Sec0(1..16)=0x4752494200000002000000000000ad9c
$ wgrib2 png.grb2 -0xSec 0 -v1
1:4:Sec0(1..16)= 47 52 49 42 00 00 00 02 00 00 00 00 00 00 ad 9c
$ wgrib2 png.grb2 -0xSec 0 -v2
1:4:Sec0(1..16)=1:47 2:52 3:49 4:42 5:00 6:00 7:00 8:02 9:00 10:00 11:00 12:00 13:00 14:00 15:ad
  16:9c

The above 3 examples show a hex dump of Section 0 using the different
verbosity levels.
```

## Usage

```
-0xSec N
N = Section Number
```

See also:
[-Sec_len](Sec_len.md),
[-checksum](checksum.md),

---

> Description: inv X Hex dump of section X (0..8)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/0xSec.html>_
