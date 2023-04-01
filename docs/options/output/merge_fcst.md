# -merge_fcst

## Introduction

The -merge_fcst option will try to combine
adjacent messages by increasing the time period for forecast average,
accumulation, minimum and maximum. For example,

```
$ wgrib2 prate.l.gdas.201404.ts -for 1:4
1:0:d=2014040100:PRATE:surface:0-1 hour ave fcst:
2:13083:d=2014040100:PRATE:surface:1-2 hour ave fcst:
3:26587:d=2014040100:PRATE:surface:2-3 hour ave fcst:
4:40437:d=2014040100:PRATE:surface:3-4 hour ave fcst:

$ wgrib2 preas.201404.ts -for 1:4 -merge_fcst 4 /tmp/all.grb
(listing of input)

$ wgrib2 /tmp/all.grb
1:0:d=2014040100:PRATE:surface:0-4 hour ave fcst:

$wgrib2 prate.l.gdas.201404.ts -for 1:4 -merge_fcst 2 /tmp/all2.grb
(iisting of input)

$ wgrib2 /tmp/all2.grb
1:0:d=2014040100:PRATE:surface:0-2 hour ave fcst:
2:29531:d=2014040100:PRATE:surface:2-4 hour ave fcst:
```

The -merge_fcst option needs the fields to be processed
in order. Fields that are not fcst averages/accumlations are ignored. In
the previous example, the input data was a time series of one varianble, and
the fields were in order. The more common case is that you have the fhr06
(0-6 hour forecast), fhr12 (6-12 hour forecast), fhr18 (12-18 hour forecast)
and fhr24 (18-24 hour forecast) files and you want the 0-24 hour mean. Each of the
forecast files contain hundreds of fields.

```
$ cat fhr06 fhr12 fhr18 fhr24 | \
   wgrib2 - -set_grib_type c1 -set_grib_max_bits 20 -set_bin_prec 20 \
   -if ":PRATE:" -merge_fcst 4 OUT \
   -if ":APCP:" -merge_fcst 4 OUT \
   -if ":ACPCP:" -merge_fcst 4 OUT \
   -if ":BGRUN:" -merge_fcst 4 OUT
```

The above command works by

1. The first line writes the grib to stdout in chrnological order

- The second line has wgrib2 read from stdin and sets the packing and precision
- The third line process the PRATE, the -merge_fcst sees the data in the proper order
- The 4-6 lines process the APCP, ACPCP and BGRUN fields

You can use the old and slow way which is

```
$ cat fhr06 fhr12 fhr18 fhr24 > IN.grb
$ wgrib2 IN.grb | sort -t: -k4,4 -k5,5 -k6,6n | wgrib2 -i IN.grb -grib OUT.grb
$ wgrib2 OUT.grb -merge_fcst 4 OUT
```

## Usage

```
-merge_fcst (N=number fields to combine) (output grib file)

When N > 0, then N fields are merged together and then written to the output grib file
   (requires wgrib2 v2.0.1)
When N == 0, then there is no limit to the number of fields to be merged
             and intermediate steps are written out
              ex. 0-1 + 1-2 + 2-3 -> 0-1, 0-2, 0-3
```

The most common use would be get the 12-hour, daily, N-day average
precipation forecasts from the N hour to N+6 hour precipitation
forecasts.

### Restrictions

1. Only 1 time range specificaion is allowed in the PDS (no nested time ranges)

- Forecast time units must be the same between message to be merged.
- The various metadata in the grib messages to be merged must be identical except for
  the averaging/accumulation interval.

See also:

---

> Description: out X Y merge forecast ave/acc/min/max X=number to intervals to merge (0=every) Y=output grib file

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/merge_fcst.html>_
