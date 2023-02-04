# wgrib2: -one_line

## Introduction

The -one_line option changes the format of the inventory
so all the items are one line. This is useful in using -grid, for example, in
making inventories that can be grepped.

Suppose we want select fields by the grid type.

```
-sh-2.05b$ ./wgrib2 eta.t00z.awphys18.grb2 -d 1 -s -grid
1:0:d=2003090300:MSLET:mean sea level:18 hour fcst:grid_template=30:
        Lambert Conformal: (614 x 428) scan WE:SN res 8
        Lat1 12.19 Lon1 226.541 Lov 265
        Latin1 25 Latin2 25 LatSP 0 LonSP 0
              North Pole (614 x 428) Dx 12.191 Dy 12.191 mode 8
```

The above format doesn't work with grep. By using the -one_line option,
we can easily extract specific grids.

```
-sh-2.05b$ ./wgrib2 eta.t00z.awphys18.grb2 -d 1 -s -grid -one\_line
1:0:d=2003090300:MSLET:mean sea level:18 hour fcst:grid_template=30:
Lambert Conformal: (614 x 428) scan WE:SN res 8 Lat1 12.19 Lon1 226.541
Lov 265 Latin1 25 Latin2 25 LatSP 0 LonSP 0       North Pole (614 x 428)
Dx 12.191 Dy 12.191 mode 8
```

## Usage

```
-one_line
      change format so that all the information/field is on one line
```

---

> Description: init puts all on one line (makes into inventory format)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/one_line.html>_
