# wgrib2: -undefine

## Introduction

The -undefine option sets the selected the decoded grid
values to undefined. The grid points are have to be inside or outside
a user defined lat-lon box. This option can be used to limit the output
when writing text output. For example, you were only interested in
the UK, you could use this option to undefine the grid points outside
the of UK. Then when you write the data in spread-sheet format, you
would get a much smaller output. This option can also be used to
find the regional average using the stat option.

## Usage

```
-undefine (in-box|out-box) lon0:lon1 lat0:lat1

in-box:  decoded grid points inside the box are set to undefined
out-box: decoded grid points outside the box are set to undefined
lon0:lon1  west-east longitudes of the box
lat0:lat1  south-north latitudes of the box

Points on the box boundary are considered to be in the box.
```

### Example

```
$ wgrib2 test.grb2 -undefine out-box 350:10 -10:10 -stats
1:0:ndata=65160:undef=64719:mean=94.1229:min=58.1:max=125.2
```

The above line calculates the statistics for the box -10W-10E 10S-10N

```
$ wgrib2 test.grb2 -undefine out-box 10:30 20:40 -undefine in-box 12:28 22:38 -bin boundary.bin
```

The above line undefines the grid points outside of a box and then undefines the grid points of a smaller
box that is contained in the first box. Then it writes the data as a binary file. The data file contains
the data points for a perimeter of a box. Why would someone want to do that? Think "horizontal boundary
conditions for a regional model". For this to work well, a module to write the data out in grib-2 needs
to be written. To work in the (i,j) coordinates, see the -ijundefine option.

See also:
[-ijundefine](./ijundefine.md),
[-spread](./spread.md),
[-stats](./stats.md),
[-undefine_val](./undefine_val.md),

---

> Description: misc X Y Z sets grid point values to undefined X=(in-box|out-box) Y=lon0:lon1 Z=lat0:lat1

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/undefine.html>_
