# wgrib2: -set_ts_dates

## Introduction

The -set_ts_dates and -set_date options
changes the reference date of the in-memory grib (sub-)message. You can write out the message
with the new date by the -grib (fast) and
-grib_out (slow) options. Of course if the in-memory
grid values have changed, you have to use the latter option.

The -set_date X option changes the date code to X.
For time series, you want the date code to increment, and you use the
-set_ts_dates X:Y:Z option. Here, X is the date code,
Y is the time increment and Z is number of fields to have the same
date code.

## Usage

```

-set_ts_dates  X:Y:Z
                    X = starting date
                    Y = time increment
                    Z = block size

                    X=YYYY, YYYYMM, YYYYMMDD, YYYYMMDDHH, YYYYMMDDHHmm, YYYYMMDDHHmmSS
                    YYYY=year, MM=month, DD=day, HH=hour, mm=minute, SS=second
                    MM=01 if MM is not specified
                    DD=01 if DD is not specified
                    HH=00 if HH is not specified
                    mm=00 if mm is not specified
                    SS=00 if SS is not specified

                    Y=IS
                      I = positive integer
                      S = year, month, day, hour, minute, second

                    Z=# fields / date code (must be one or greater)


```

### Example

```

# date code increments by 1 hour
$ wgrib p.grb -set\_ts\_dates 2010020304:1hour:1
1:0:d=2010020304:PRMSL:mean sea level:anl:
2:17960:d=2010020305:PRSML:mean sea level:anl:
3:34248:d=2010020306:PRMSL:mean sea level:anl:
4:48055:d=2010020307:PRMSL:mean sea level:anl:

```

See also:

[-grib](grib.html),
[-grib_out](grib_out.html),

---

> Description: misc X Y Z changes date code for time series X=YYYYMMDDHH(mmss) Y=dtime Z=#msgs/date

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_ts_dates.html>_
