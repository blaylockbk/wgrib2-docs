# -csv_long (comma separated values)

## Introduction

The -csv_long option writes the grid values to a specified
file as a comma separated values (text) which can be imported into a
spread sheet. This function is similar to
-spread and -csv
with a different output format. The format is

```
   "time0","time1","time2","field","ftime","level",longitude,latitude,grid-value
```

Some programs have a simple data model: x, y, z and time. Forecast models
need a more complicated data model. Most forecasts only need
3 time coordinates. Some examples that need 1 to 3 times are,

```
  time0: reference time
         ex. temperature analysis for 00Z, June 2, 2014

  time0, time1: reference time, forecast time
         ex. temperature forecast started with initial conditions at 00Z, June 2, 2014
             and verifing at 00Z, June 3, 2014

  time0, time1, time2: reference time, start of forecasting period, end of forecasting period
         ex. precipitation forecast started with initial conditions at 00Z, June 2, 2014
             and for the average precipitation from 00Z June 2, 2014 to 00Z June 3, 2014.
```

You can define quantities that require more than 3 times but they are
relatively rare.

```
  Examples of quantities that need more than 3 times to describe
   a) climatology of 6-12 hour forecasts (start time of climatology, end time of the
         climatology, 6 hour, 12 hour)
   b) forecast of the monthly mean of the average 00Z-01Z temperature. (starting time
      of the monthly average, ending time of the monthly average, starting time of
      the diuranl average, ending time of the diurnal average)
```

For an analysis that is valid at a time0, time1 and time2 will
have the same value as time0. For a forecast valid at a
specific time, time0 will be the start of the forecast and
time1 will be the valid time. Time2 will have the same value
as time1. Finally for a forecast that is an average, accumulation,
minimum value or maximum value of a time interval, time0
will be the start of the forecast, time1 will be the start
of the time interval and time2 will the end of the interval.
For more complicated quantities, see the ftime value.

In wgrib2 speak,

```
   time0 is -T in a different format
   time1 is -start\_FT in a different format
   time2 is -end\_FT in a different format
   field is -var in a different format
   ftime is -ftime
   level is -lev
   longitude is in degrees with values between -180 and 180
   latitude is in degrees values between -90 and 90 (south = -ve, north = +ve)
```

The -csv_long option only works on the grids
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
Since -csv_long doesn't print undefined
grid points, the CSV files is much smaller.

### Extended Variable Names

The default field value (see above) is the grib name such as TMP or HGT.
However, the grib name may not be unique. For example, the field could be
the HGT from the 19th ensemble member. A better field name may be
"HGT.ENS=+19". You enable the extended variable name by adding the option
-set_ext_name 1.

## Usage

```
-csv_long output_file_name
   The CSV is written to output_file_name, note output_file_name cannot be a memory file
-set_ext_name 1 -csv output_file_name
   the field is the extended grib name
```

### Warning #1

The options -csv,
-csv_long,
-spread and
-text do not support memory files.
You can blame sloth or lack of need. I like to think that
text files with grid point values are insanely large
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
[-csv](./csv.md),
[-text](./text.md),
[-bin](./bin.md),
[-ieee](./ieee.md),
[-match](./match.md),
[-set_ext_name](./set_ext_name.md),
[-undefine](./undefine.md)

---

> Description: out X make comma separated file, X=file (WxText enabled)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/csv_long.html>_
