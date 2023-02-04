
### wgrib2: -ncep\_norm



### Introduction



The GFS model, for example, produces precipitation with a
hard-to-use time step. If you gathered the precip forecasts
from the GFS, it would look like,


```

$ wgrib2 apcp.grb
1:0:d=2009042400:APCP:surface:0-3 hour acc fcst:
2:3340:d=2009042400:APCP:surface:0-6 hour acc fcst:
3:5534:d=2009042400:APCP:surface:6-9 hour acc fcst:
4:7239:d=2009042400:APCP:surface:6-12 hour acc fcst:
5:9304:d=2009042400:APCP:surface:12-15 hour acc fcst:
6:12378:d=2009042400:APCP:surface:12-18 hour acc fcst:

```


If you obtained the 6 hourly forecasts, everthing would be
quite useful. But if you obtained the 3 hourly forecasts, you
would notice the averaging period is either 3 or 6 hours. To
create 3-hour accumulations, you can use the 
-ncep\_norm option.


```

$ wgrib2 apcp.grb -ncep\_norm new\_apcp.grb
1:0:d=2009042400:APCP:surface:0-3 hour acc fcst:
2:3340:d=2009042400:APCP:surface:0-6 hour acc fcst:
3:5534:d=2009042400:APCP:surface:6-9 hour acc fcst:
4:7239:d=2009042400:APCP:surface:6-12 hour acc fcst:
5:9304:d=2009042400:APCP:surface:12-15 hour acc fcst:
6:12378:d=2009042400:APCP:surface:12-18 hour acc fcst:
7:14396:d=2009042400:APCP:surface:18-21 hour acc fcst:
$
$ wgrib2 new\_apcp.grb
1:0:d=2009042400:APCP:surface:0-3 hour acc fcst:
2:7769:d=2009042400:APCP:surface:3-6 hour acc fcst:
3:13271:d=2009042400:APCP:surface:6-9 hour acc fcst:
4:18773:d=2009042400:APCP:surface:9-12 hour acc fcst:
5:24275:d=2009042400:APCP:surface:12-15 hour acc fcst:
6:32044:d=2009042400:APCP:surface:15-18 hour acc fcst:
7:38301:d=2009042400:APCP:surface:18-21 hour acc fcst:

```

 The -ncep\_norm option is not limited
to 3 hour intervals. It can be used with the CFSv2 which has
1 hour intervals. Some restrictions are:

1. time units of forecast time and statistical processing must be the same
- only works with averages and accumulations (Code Table 4.10)
- the grid must be the same
- the meta-data must be the same except for the forecast timing.
- Does not work for CRAIN, CSNOW, CICEP, CFRZR


### Usage




```

-ncep_norm output_file

```

### Conditions



The input grib file must have a very specific format.
Each field must be grouped together in time series format.
That is because the program scans the file in sequential order.
Of course, you can alter the order by the -i
option. For example,


```

$ wgrib2 -match ':APCP:' mixed\_up -vt | sort -t: -k3,3 | \
 wgrib2 -i mixed\_up -ncep\_norm new\_apcp.grb
1:0:d=2009042400:APCP:surface:0-0 day acc fcst:
6:26940:d=2009042400:APCP:surface:0-3 hour acc fcst:
11:56805:d=2009042400:APCP:surface:0-6 hour acc fcst:
16:85235:d=2009042400:APCP:surface:6-9 hour acc fcst:
51:281962:d=2009042400:APCP:surface:6-12 hour acc fcst:
61:338477:d=2009042400:APCP:surface:12-15 hour acc fcst:
66:367933:d=2009042400:APCP:surface:12-18 hour acc fcst:
71:396448:d=2009042400:APCP:surface:18-21 hour acc fcst:
...
$ wgrib2 new\_apcp.grb
1:0:d=2009042400:APCP:surface:0-3 hour acc fcst:
2:7769:d=2009042400:APCP:surface:3-6 hour acc fcst:
3:13271:d=2009042400:APCP:surface:6-9 hour acc fcst:
4:18773:d=2009042400:APCP:surface:9-12 hour acc fcst:
5:24275:d=2009042400:APCP:surface:12-15 hour acc fcst:
6:32044:d=2009042400:APCP:surface:15-18 hour acc fcst:
7:38301:d=2009042400:APCP:surface:18-21 hour acc fcst:
...

```

### Using GFS forecast files



The typical GFS forecast file has a name like,

```

GFS:
    gfs.t(HH)z.(type).(resolution).f(fhour)

    gfs.t06z.pgrb2b.0p25.f006
    gfs.t18z.pgrb2.0p25.f006

```

So to get the "normalized" APCP, you can do


```

   $ test.sh

where test.sh is

#!/bin/sh
#
# example of calculating GFS APCP for custom periods
#  want 06-09, 09-12, 12-15, 15-24, 24-48 hours
#  need the 06, 09, 12, 15, 24 and 48 hour forecasts
#
# the output iis in apcp.grb
# the calculated APCP fields may have a small negative because of
# finite precision is used

dir=/gpfs/dell1/nco/ops/com/gfs/prod/gfs.20211212/00/atmos/

cat $dir/gfs.t00z.pgrb2.0p25.f006 $dir/gfs.t00z.pgrb2.0p25.f009 \
      $dir/gfs.t00z.pgrb2.0p25.f012 $dir/gfs.t00z.pgrb2.0p25.f015 \
      $dir/gfs.t00z.pgrb2.0p25.f024 $dir/gfs.t00z.pgrb2.0p25.f048 | \
wgrib2 - -match ":APCP:" -match ":0-[0-9]* [a-z]* acc fcst:" -set_grib_type c1 -ncep_norm apcp.grb


The above script uses several techniques to speed up the processing.

"cat (list of files)"   This writes the grib files to stdout.  This avoids a temporary disk file.

"wgrib2 -"   This reads the grib file from stdin

"-match "(regex)"    This avoids decoding grib messages that do not match 
             the regular expression

```


See also: 














----

>Description: out   X      normalize NCEP-type ave/acc X=output grib file

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ncep_norm.html>_