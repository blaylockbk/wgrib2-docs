# wgrib2: -start_ft, -start_FT, -end_ft, -end_FT

## Introduction

Suppose you make a forecast starting from initial conditions of 00Z January 1 for the average
conditions from the month of February. In this forecast, there are three time stamps.

```

   Name                        date           YYYYMMDDHH option  (HH)Z(DD)(MON)YYYY option      with minutes and seconds

   reference time              00Z Jan 1     -t                  -v2 -t                         -T
   start of forecast period    00Z Feb 1     -start_ft           -v2 -start_ft                  -start_FT
   end of forecast period      00Z Mar 1     -end_ft             -v2 -end_ft                    -end_FT

```

For common ftimes,

```


analysis:                      reference time == start_ft == end_ft
6 hour forecast:               reference time + 6 == start_ft == end_ft
30 year monthly climatology:   reference time = start_ft = start of 1st month used to make climatology
                               end_ft = end of the last month used to make the climatology
## Usage



```

-start_ft
-end_ft
-start_FT
-end_FT

```


### Example





```

$ wgrib2 a.grb -start_ft
1:0:start_FT=20111101000000

```


See also:



```

---

> Description: inv verf time = reference_time + forecast_time (YYYYMMDDHHMMSS) - no stat. proc time

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/start_FT.html>_
