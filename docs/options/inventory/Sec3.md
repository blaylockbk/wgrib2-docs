# wgrib2: -Sec3

## Introduction:

The -Sec3 option prints a short summary of Section 3,
the Grid Definition Section.

```
$ wgrib2 png.grb2 -Sec3
1:4:Sec3 len=72 src gdef=0 npts=65160 Grid Def Template=3.0 opt arg=0

len=72                        Section 3 is 72 octets/bytes in length
src gdef=0                    Source of grid definition, Code Table 3.0
npts=65160                    Number of data points
Grid Def Template=3.0         Grid definition template number, Code Table 3.1
opt arg=0                     optional arguments
```

## Usage

```
-Sec3
```

See also:

---

> Description: inv contents of section 3 (Grid Definition Section)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/Sec3.html>_
