# wgrib2: -last

## Introduction

The -last FILE option writes the results of the previous option
to FILE. The FILE can be a disk file, temporary file or memory file. If
the -last option preceeds any inventory options, then the
"grib message number[.submessage number:byte location" will be written to the file.

The -last option was designed for callable wgrib2 to obtain
inventory information.
Note the -s_out FILE option should be replaced by the
more powerful -s -last FILE syntax.

The -last option does not write to the inventory, so
if you have two consecutive -last options, the second
-last will have no output. With wgrib2 v3.0.0+, the
-last and -last0 options will
not clear the last option output buffer.
So the the second -last option will have the same output
as the immediately preceeding -last or -last0
option.

## Usage

```

-last FILE

```

### Example

Suppose you want a the grid values (nearest neighbor) for 1000 points. You could
do something like this,

```

$ wgrib2 gep19.t00z.pgrb2af180 -s -lon 0 10 > point1.txt
$ wgrib2 gep19.t00z.pgrb2af180 -s -lon 20 50 > point2.txt
...
$ wgrib2 gep19.t00z.pgrb2af180 -s -lon 250 40 > point1000.txt

```

This would not be the fastest because you have to read and decode
the input file 1000 times. You could read and decode the file once
by using the -last option. Here how to do it using N=2.

```

wgrib2 gep19.t00z.pgrb2af180 -s -last point1.txt -last point2.txt \
    -print_out ':' point1.txt -lon 0 0 -last point1.txt -nl_out point1.txt \
    -print_out ':' point2.txt  -lon 10 20 -last point2.txt -nl_out point2.txt

```

The -new_grid option can interpolate to set of
user defined grid points.

See also:
[-last0](./last0.html)
[-new_grid](./new_grid.html)

---

> Description: inv> X write last inv item to file X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/last.html>_
