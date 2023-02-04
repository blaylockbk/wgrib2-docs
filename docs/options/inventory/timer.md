# wgrib2: -timer (requires wgrib2 v3.0.0+)

## Introduction

The -start_timer
and -timer options were designed for developers
and people who want to know how fast wgrib2 is running. Most users
can ignore these options.

The -timer option prints out the
time in seconds since the last -start_timer
or -timer
option was executed. The -timer option is
executed in the initialization phase (no output) and the
finalization phase (extra output). The timer is active
in the finalization phase in order to time the finialization of
options.

This option uses the OpenMP funcion omp_get_wtime(), and require
compilation with OpenMP.

## Usage

```

-start_timer
   sets timer_time=0
-timer
   prints timer_time in seconds.
   sets timer_time = 0;
   at end of processing, prints the average if these results

  note: there is only one instance of timer_time
        timer_time is set to 0 in the initialization of wgrib2

```

### Example

```

$ wgrib2 gep19.aec -s -start\_timer -csv junk.csv -timer
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19:time=0.068086
2:70707:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19:time=0.064850
3:96843:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19:time=0.055596
...
51:2278237:d=2009060500:PRMSL:mean sea level:180 hour fcst:ENS=+19:time=0.065116
finalize-time=0.000015:ave_time=0.061512 count=51

Writing of the CSV is taking about 0.06 seconds per field.

```

See also:
[-alarm](./alarm.md),
[-start_timer](./start_timer.md),

---

> Description: inv reads OpenMP timer

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/timer.html>_
