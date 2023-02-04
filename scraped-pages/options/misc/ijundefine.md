# wgrib2: -ijundefine

## Introduction

The -ijundefine option sets the
selected grid values to undefined. The grid points are have to be
inside or outside a user defined (i,j) box. I and j are the column
and row number of the "raw" data starting from 1. This option can be
used to limit the output when writing output.
For example, you were only interested in the UK, you could use this
option to undefine the grid points outside the of UK. Then when you write
the data in spread-sheet format, you would get a much smaller output.
This option can also be used to find the regional average using the
stat option. Note that the
-ijundefine option changes the in-memory values
of the grid points. If you want to alter the grib file, you will
have to write out the in-memory grid point values using the
the -grib_out option.

## Usage

```

-ijundefine (in-box|out-box) ix0:ix1 iy0:iy1

in-box:  decoded grid points inside the box are set to undefined
out-box: decoded grid points outside the box are set to undefined
ix0:ix1 columns limits (1 <=  ix0 <= ix1 < nx)
iy0:iy1 row limits (1 <= iy0 <= iy1 <  ny)
Note: the order of the data should be in default mode (we:sn).
Note: ix0, iy0 is the lower left hand corner (w/s).

Points on the box boundary are considered to be in the box.

```

### Example

```

$ wgrib2 test.grb2 -ijundefine out-box 1:1 1:10 -stats

```

The above line calculates the statistics for ix=1, iy=1..10.

```

$ wgrib2 test.grb2 -ijundefine out-box 10:30 20:40 -ijundefine in-box 11:29 21:39 -bin boundary.bin

```

The above line undefines the grid points outside of a box and then undefines the grid points of a smaller
box that is contained in the first box. Then it writes the data as a binary file. The data file contains
the data points for a perimeter of a box. Why would someone want to do that? Think "horizontal boundary
conditions for a regional model". For this to work well, a module to write the data out in grib-2 needs
to be written. BTW the binary file will compress to an extremely small file.

See also:
[-undefine](./undefine.html),
[-spread](./spread.html),
[-stats](./stats.html),
[-undefine_val](./undefine_val.html),

---

> Description: misc X Y Z sets grid point values to undefined X=(in-box|out-box) Y=ix0:ix1 Z=iy0:iy1 ix=(1..nx) iy=(1..ny)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ijundefine.html>_
