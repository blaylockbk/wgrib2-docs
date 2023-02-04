# wgrib2: -Sec0

## Introduction:

The -Sec0 option prints a short summary of Section 0,
the Indicator Section.

```

$ wgrib2 png.grb2 -Sec0
1:4:Sec0=GRIB reserved 0x0000 Discipline=0 Grib_Edition=2 len=44444

GRIB                          octets 1-4, should be GRIB
reserved 0x0000               octects 5-6, 0x0000 if not used
Discipline=0                  Discipline, Table 0.0
Grib_Edition=2                Edition, 2 for grib version2
len=44444                     length in octets/bytes of entire grib message

```

## Usage

```

-Sec0

```

See also:

---

> Description: inv contents of section0

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/Sec0.html>_
