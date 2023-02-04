# wgrib2: -csv (comma separated values)

## Introduction

The -csv option writes the grid values to a specified
file as a comma separated values (text) which can be imported into a
spread sheet. This function is similar to -spread
and -csv_long with a different output format.

```

   "time0","time1","field","level",longitude,latitude,grid-value

```

For an analysis that is valid at a specific time, time0 and time1
will have the same value. For a temporal average or accumulation
of analyses, time0 will be the start and time1 will be end of
the average or accumulation.

For a forecast, time0 will be the start of the forecast (i.e, date
of the initial conditions). For a forecast for a specific time,
then time1 will be time that the forecast is valid. For a forecast
of a temporal average or accumulation, time1 wll be the end of the
average or accumulation.

In grib speak, time0 is the reference time and time1 is
the verification time. Here are some examples,

```

1:0:d=2009062900:TMP:surface:anl:     (default inventory)

   This an analysis (anl) of the surface temperature.
   This analysis is valid for 2009062900.
   Time0 = 2009062900    Time1 = 2009062900
   field=TMP             level=surface

29:1155322:d=2009060500:UGRD:925 mb:180 hour fcst:ENS+19     (default inventory)
   This is a 180 hour forecast of the 925 hPa zonal wind.
   The forecast run uses initial conditions at 2009060500
   and verifies 180 hours laster at 2009061212.
   Time0 = 2009060500    Time1 = 2009060500
   field=UGRD            level = 925 mb

43:1844451:d=2009060500:APCP:surface:174-180 hour acc fcst:ENS=+19     (default inventory)
   This is a forecast of accumulated precipitation from 174 to 180 hours.
   The forecast run uses initial conditions at 2009060500 and
   accumulates the precip from 2009061206 to 2009061212.  The
   verification time is defined as the end of the accumulation period
   Time0 = 2009060500    Time1 = 2009060500
   field= APCP           level= surface

```

The -csv option only works on the grids
for which wgrib2 can derive latitude and longitudes values.
Otherwise no output is generated. The undefined value is 9.999e20.

The size of a CSV file can be overwhelming. One technique is to
only generate the CSV for variables that you need. This can be
done with the -match option. Even with
the -match option, the high-resolution
models can stil generate huge files. The next technique is
to undefine the grid points that you don't want. For example,
you are only interested in Hawaii (its 0C outside as I write
this). Then you can use the the -undefine option
to set the grid points outside of the Hawaii domain to undefined.
Since -csv doesn't print undefined
grid points, the CSV files is much smaller.

### Extended Variable Names

The default field value (see above) is the grib name such as TMP or HGT.
However, the grib name may not be unique. For example, the field could be
the HGT from the 19th ensemble member. A better field name may be
"HGT.ENS=+19". You enable the extended variable name by adding the option
-set_ext_name 1.

## Usage

```

-csv output_file_name
   the field is the grib name
   output_file_name cannot be a memory file
-set_ext_name 1 -csv output_file_name
   the field is the extended grib name
   output_file_name cannot be a memory file

```

### Example 1

```

$ wgrib2 fcst.grb2 -csv junk
1:0:d=2007032600:HGT:1000 mb:anl:
2:125535:d=2007032600:HGT:1000 mb:3 hour fcst:
$ cat junk
"2007-03-26 00:00:00","2007-03-26 00:00:00","HGT","1000 mb",0,-90,164.1
"2007-03-26 00:00:00","2007-03-26 00:00:00","HGT","1000 mb",0.5,-90,164.1
"2007-03-26 00:00:00","2007-03-26 00:00:00","HGT","1000 mb",1,-90,164.1
"2007-03-26 00:00:00","2007-03-26 00:00:00","HGT","1000 mb",1.5,-90,164.1
...
"2007-03-26 00:00:00","2007-03-26 03:00:00","HGT","1000 mb",-1.5,90,-91.7
"2007-03-26 00:00:00","2007-03-26 03:00:00","HGT","1000 mb",-1,90,-91.7
"2007-03-26 00:00:00","2007-03-26 03:00:00","HGT","1000 mb",-0.5,90,-91.7

```

### Example 2: CSV for one point

Suppose we want a CSV for one point. You use the -undefine option
to set the to undefined except for the selected point. Suppose we have a 1x1 grid.

```

$ wgrib2 gep19.aec   -undefine out-box .9:1.1 10.9:11.1 -csv  1E11N.csv
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:70707:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
3:96843:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
...
$ cat 1E11N.csv
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",1,11,12440.8
"2009-06-05 00:00:00","2009-06-12 12:00:00","TMP","200 mb",1,11,219.6
"2009-06-05 00:00:00","2009-06-12 12:00:00","RH","200 mb",1,11,100
...

```

### Example 3: CSV for two points

The simple way is do the previous example twice. There is a computationally
faster method.

```

$ wgrib2 gep19.aec -rpn sto_1 -undefine out-box .9:1.1 10.9:11.1 -csv junk -rpn rcl_1 -undefine out-box 1.9:2.1 19.9:20.1 -csv junk
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:70707:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
3:96843:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
...
$ cat junk
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",1,11,12440.8
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",2,20,12360.8
"2009-06-05 00:00:00","2009-06-12 12:00:00","TMP","200 mb",1,11,219.6
"2009-06-05 00:00:00","2009-06-12 12:00:00","TMP","200 mb",2,20,218.8
...

```

### Warning #1

The options -csv,
-csv_long,
-spread and
-text do not support memory files.
You can blame sloth or lack of need. I like to think that
text files with grid point values are are insanely large
and shouldn't be saved in memory.

### Warning #2

It may be tempting to take a grib file, convert it into a CSV file
and then deal with the CSV file. After all, everybody can read
a CSV file. Sure there is a litte overhead of reading a CSV file
but who cares. Suppose you want to read some GFS forecasts files
(20 forecast times, 5 days every 6 hours) at 0.25 x 0.25 degree global resolution.
Your CSV file is going to be about 720 GBs. Suppose that our hard
drive can write/read at 70 MB/s. Then we are talking about 3 hours to
write the CSV file and 3 hours to read the CSV file not including CPU time
which will slow down the process. Converting grib into CSV is a
viable strategy if the conversion is limited. You need to restrict
the number of fields converted and should consider only converting
a regional domain. Note, I wrote "viable" and not optimal.

See also:
[-spread](./spread.md),
[-csv_long](./csv_long.md),
[-text](./text.md),
[-bin](./bin.md),
[-ieee](./ieee.md),
[-set_ext_name](./set_ext_name.md),
[-undefine](./undefine.md)

---

> Description: out X make comma separated file, X=file (WxText enabled)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/csv.html>_
