# wgrib2: -AAIG

### Ascii ArcInfo Grid

## Introduction

The -AAIG option writes the data into a Ascii ArcInfo Grid file.
This option is experimental and only supports equally spaced lat-lon grids.
My reading of the format specifications that each file contains a single grid.
Therefore, each field is written to different file (\*.asc) which
is written to the current directory.

### File name convention for ouput: \*.asc

The file name convention

```
   NAME = grib name (-var), ex. TEMP, HGT
   LEVEL = level, ex. surface, 2_m_above_ground, 500_mb
   RT = reference time YYYYMMDDHH
   VT = verification time (end_ft) YYYYMMDDHH

   If RT is the same as VT
     output = NAME.LEVEL.RT.asc

   If RT is different than VT
     output = NAME.LEVEL.RT.VT.asc
```

### Problems with file name convention

The above file name convention works for a simple GFS forecast.
However, life quickly gets more complicated and a new file name
convention was needed (-AAIGlong).

## Usage

```
-AAIG
```

### Example

```
wgrib2 in_file -match ':HGT:400 mb:' -AAIG
```

The above line converts all the 400 mb HGT fields into an
arcinfo ascii grid file.

See also: [-AAIGlong](./AAIGlong.md),
[-csv](./csv.md)

---

> Description: out writes Ascii ArcInfo Grid file, lat-lon grid only (alpha)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/AAIG.html>_
