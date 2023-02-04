# wgrib2: -spread

## Introduction

The -spread option writes the grid values to a specified
file as a comma separated values (text) which can be imported into a spread sheet.

```

   lon,lat,(VARIABLE DESCRIPTION)
   lon-1,lat-1,val-1
   lon-2,lat-2,val-2
   ...
   lon-N,lat-N,val-N

```

The latitudes and longitudes are only available for supported grids
and only the default WE:SN order is supported. The undefined value is 9.999e20.

Use the -undefine option to limit the output to
specified regions.

## Usage

```

-spread output_file_name

```

### Example

```

$ wgrib2 test.grb2 -s | grep ":RH:2 m" | wgrib2 -i test.grb2 -spread data.txt
285:36796469:d=2005090200:RH:2 m above ground:60 hour fcst

```

The above line extracts the 2 meter RH from file test.grb2 and writes it in data.txt.

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
[-csv](./csv.html),
[-netcdf](./netcdf.html),
[-text](./text.html),
[-bin](./bin.html),
[-ieee](./ieee.html),
[-undefine](./undefine.html)

---

> Description: out X write text - spread sheet format into X (WxText enabled)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/spread.html>_
