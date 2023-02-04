# wgrib2: -last0

## Introduction

The -last0 FILE option writes the results of the previous option
to the beginning of FILE. The FILE can be a disk file, temporary file or memory file. If
the -last option preceeds any inventory options, then the
"grib message number[.submessage number:byte location" will be written to the file.

The -last0 option was designed for callable wgrib2. This
allows calls to wgrib2 to obtain inventory information. Note
the -s_out FILE option should be replaced by the
more powerful -s -last FILE syntax.

The -last0 option does not write to the inventory, so
if you have two consecutive -last0 options, the second
-last0 will have zero output. With wgrib2 v3.0.0+, the
-last and -last0 options will
not clear the last option output buffer.
So the the second -last0 option will have the same output
as the immediately preceeding -last or -last0
option.

## Usage

```

-last0 FILE

```

See also:
[-last](./last.md)

---

> Description: inv> X write last inv item to beginning of file X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/last0.html>_
