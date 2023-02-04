# wgrib2: -d

## Introduction

The -d N option specifies the grib message number (N) to
process. (The -d N option comes from wgrib where "d" stood fom "dump".)
If Nth message has submessage, then the first submessage is chosen (N.1).
For messages with submessages, use -d N.M to select the
Mth submessage of the Nth message. If M is missing, only the first submessage is selected.

With wgrib2 v3.0.0, you can add an optional offset. For example, -d N:OFFSET
will skip OFFSET bytes (must be positive), find the next grib message, give it a label N, and dump it.
The other form, -d N.M:OFFSET will skip OFFSET bytes (must be positive),
find the next grib message and dump the Mth submessage of that grib message.
Using the offset option can be faster for large files, and can be used to skip sections of a grib
file that stop wgrib2 processing.
The -d option works for files and pipes.

If there are more than one -d on a command line, only
the last one is used.

The form, -d all, is not valid in wgrib2. This
is the default action.

The -d should be used with caution. The order of
messages records within grib files can change without notice (ex. today's forecast
may have a different order from tomorrow's forecast). I use
-d interactively but this option should not be used
in scripts unless you are 100% certain of the order of the records.

## Usage

```

-d N
-d N.M
-d N:OFFSET
-d N.M:OFFSET
where N is an integer larger than 0, M is an positive integer, OFFSET is a positive integer

```

### Example

```

$ wgrib2 test.grb2 -s -d 1 -bin data.bin
1:0:d=2005090200:HGT:1000 mb:60 hour fcst
The above command writes the first record to a binary file data.bin


$ wgrib2 test.grb2 -s -d 287.2 -bin data.bin
287.2:37032193:d=2005090200:VGRD:10 m above ground:60 hour fcst
The above command dumps the second submessage of record 287.

$ cat gfs.t00z.master.grb2f048 | wgrib2 - | head -n 4
1:0:d=2018030400:PRES:mean sea level:48 hour fcst:
2:3565800:d=2018030400:REFC:entire atmosphere:48 hour fcst:
3:4748163:d=2018030400:VIS:surface:48 hour fcst:
4:5462029:d=2018030400:UGRD:planetary boundary layer:48 hour fcst:
$ cat gfs.t00z.master.grb2f048 | wgrib2 - -d 3:4748163
3:4748163:d=2018030400:VIS:surface:48 hour fcst:
$ cat gfs.t00z.master.grb2f048 | wgrib2 - -d 1:4748163
1:4748163:d=2018030400:VIS:surface:48 hour fcst:

Using the offset option on a pipe

```

See also:
[-for](./for.md),
[-i](./i.md),

---

> Description: init X dump message X = n, n.m, n:offset, n.m:offset, only 1 -d allowed

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/d.html>_
