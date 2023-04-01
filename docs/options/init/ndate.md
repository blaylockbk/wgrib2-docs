# -ndate (v2.0.8)

## Introduction

The NCEP utility, ndate, will add or subtract hours from a date code.
Wgrib2 already has routines to do these calculations. So instead of
porting the ndate fortran code and the NCEP libraries, how about
a simple-to-write option that would allow wgrib2 to do the same
calculation (and more)? In my case, installing wgrib2 is a probably a given.
So adding -ndate saves me time.

The ndate utility allows you to add or subtract a integer number of hours
from a YYYYMMDDHH date code,
The -ndate option allows you to add or
subtract an integer number of minutes, hours, days, months or years from
a date code. The output format has the precision necessary for the input date code
and offset. Sure you could do all this with the gnu date program but
the -ndate option is easier to use.

The feature of the ndate utility that is not in wgrib2, is the ability
to print the current UTC date code when there is no argument. Of course, you
can do the same by "date -u +%Y%m%d%H". For users of the ndate utility, note that
the order of arguments are reversed between the utility and wgrib2 option.

The -ndate option is an odd
option because it does its output in the initialization phase.
In order to to trigger an "missing input file" error, you need
to run wgrib2 on a valid file. For linux/unix systems, using
the file /dev/null is a convenient, always present file.

## Usage

```
-ndate DATE OFFSET
       DATE = YYYY, YYYYMM, YYYYMMDD, YYYYMMDDHH, YYYYMMDDHHmm, YYYYMMDDHHmmss
       OFFSET=(integer)(yr|mo|dy|hr|mn)
                integer can be positive or negative
                yr = year  (0000..9999)
                mo = month (01..12)
                dy = day   (01..31)
                hr = hour  (00..23)
                mn = minute (not to be confused with month)  (00..59)

      The output format has the precision to reflects the maximum precision of
      the date code and the offset

     Priority of the output format of the date code
   output date code: YYYYMMDDHHmmss      if input date code is YYYYMMDDHHmmss
   output date code: YYYYMMDDHHmm        if input date code is YYYYMMDDHHmm   or OFFSET is in minutes (mn)
   output date code: YYYYMMDDHH          if input date code is YYYYMMDDHH     or OFFSET is in hours (hr)
   output date code: YYYYMMDD            if input date code is YYYYMMDD       or OFFSET is in days (dy)
   output date code: YYYYMM              if input date code is YYYYMM         or OFFSET is in months (mo)
   output date code: YYYY                if input date code is YYYY           or OFFSET is in years (yr)
```

### Example

```
$ wgrib2 /dev/null -ndate 2016010212 -6hr
2016010206
```

See also: [-ndates](./ndates.md)

---

> Description: init X Y X=date Y=dt print date + dt

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ndate.html>_
