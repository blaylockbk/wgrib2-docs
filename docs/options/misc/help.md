# -help

## Introduction

The -help options list the commands that match your query. For example,
suppose you want options that deal with time, then you would use the "-help time" option.

```
$ wgrib2 -help time
wgrib2 v0.2.0.3beta3 10/2015 Wesley Ebisuzaki, Reinoud Bokhorst, John Howard, Jaakko Hyvätti, Dusan Jovic,
Kristian Nilssen, Karl Pfeiffer, Pablo Romero, Manfred Schwarb, Arlindo da Silva, Niklas Sondell, Sergey Varlamov
   stock build
 -code_table_1.2  inv         code table 1.2 significance of reference time
 -code_table_4.11 inv         code table 4.11 (first) type of time intervals
 -code_table_4.11s inv         code table 4.11 (all) type of time intervals
 -end_ft          inv         verf time = reference_time + forecast_time + stat. proc time (YYYYMMDDHH) (same as -vt)
 -end_FT          inv         verf time = reference_time + forecast_time + stat. proc time (YYYYMMDDHHMMSS) (same as -VT)
 -ftime           inv         forecast time
 -MM              inv         reference time MM
 -pds_fcst_time   inv         fcst_time(1) in units given by pds
 -RT              inv         type of reference Time
 -start_ft        inv         verf time = reference_time + forecast_time (YYYYMMDDHH) : no stat. proc time
 -start_FT        inv         verf time = reference_time + forecast_time (YYYYMMDDHHMMSS) - no stat. proc time
 -t               inv         reference time YYYYMMDDHH, -v2 for alt format
 -T               inv         reference time YYYYMMDDHHMMSS
 -unix_time       inv         print unix timestamp for rt & vt
 -verf            inv         simple inventory using verification time
 -vt              inv         verf time = reference_time + forecast_time, -v2 for alt format
 -VT              inv         verf time = reference_time + forecast_time (YYYYMMDDHHMMSS)
 -YY              inv         reference time YYYY
 -count           misc        prints count, number times this -count was processed
 -end             misc        stop after first (sub)message (save time)
 -quit            misc        stop after first (sub)message (save time)
 -set_ftime       misc X      set ftime
 -set_ts_dates    misc X Y Z  changes date code for time series X=YYYYMMDDHH(mmss) Y=dtime Z=#msgs/date
 -ave             out  X Y    average X=time step, Y=output grib file needs file is special order
 -fcst_ave        out  X Y    average X=time step, Y=output grib file needs file is special order
 -nc_grads        init        require netcdf file to be grads v1.9b4 compatible (fixed time step only)
 -nc_nlev         init X      netcdf, X = max LEV dimension for {TIME,LEV,LAT,LON} data
 -nc_time         init X      netcdf, [[-]yyyymmddhhnnss]:[dt{s[ec]|m[in]|h[our]|d[ay]}], [-] is for time alignment only
 -no_nc_grads     init        netcdf file may be not grads v1.9b4 compatible, variable time step
 -no_nc_time      init        netcdf, disable previously defined initial or relative date and time step
```

## Usage

```
-help search-term
-help all
```

### Example

```
$ wgrib2 -help speed
wgrib2 v0.2.0.5beta5 6/2016  Wesley Ebisuzaki, Reinoud Bokhorst, John Howard, Jaakko Hyvätti, Dusan Jovic, Daniel Lee,
Kristian Nilssen, Karl Pfeiffer, Pablo Romero, Manfred Schwarb, Gregor Schee, Arlindo da Silva, Niklas Sondell,
Sam Trahan, Sergey Varlamov
   stock build
 -mysql_speed     out  7 args H=[host] U=[user] P=[password] D=[db] T=[table] W=[western_lons:0|1] PV=[remove unlikely:0|1]
 -wind_speed      out  X      calculate wind speed, X = output gribfile (U then V in datafile)
```

See also:

---

> Description: misc X help [search string|all], -help all, shows all options

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/help.html>_
