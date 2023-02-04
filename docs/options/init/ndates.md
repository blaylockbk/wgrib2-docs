# wgrib2: -ndates (v2.0.8+)

## Introduction

The -ndates option creates a list of date codes
using a do-loop syntax. This option has nothing to do with grib, but
adding it to wgrib2 was trivial, and this option has been so helpful
in scripting. Anyways, if a Swiss army knife can have a
bottle opener, wgrib2 can have a date code routine. Anyways one less
program to port is helpful.

Note that until wgrib2 v3.0.3, the number of date codes was limited
by a buffer size.

The -ndate and -ndates options
are initialization routines. They are run when all the options are
initialized. Therefore they are run before reading the grib file.
The -ndates_fmt option has to preceed the
-ndates option to alter the output format.

The wgrib2 command line can have multple -ndates options.
Combining -ndate and -ndates options
is more difficult because the default formatting will join the date codes.

```

-ndates DATE DATE2 DT2
-ndates DATE DT1 DT2

If DT1 is given, then DATE2=DATE+DT1

    C-meta:          for (date=DATE; date < DATE2; date += DT2) print date;
    Fortran-meta:    date=DATE
                     while (date < DATE2)
                        print date
                        date=date+DT2
                     endwhile

DATE=integer of the form YYYY, YYYYMM, YYYYMMDD, YYYYMMDDHH, YYYYMMDDHHmm, YYYYMMDDHHmmss
                note: leading zeros cannot be replaced by spaces
                YYYY = year (0000..9999), there were changes in the calendar (ex in 1752) which are ignored
                MM = month (01..12)
                DD = day (01..31)
                HH = hour (00..23)
                mm = minute (00..59)
                ss = second (00..59), there are no provisions for leap seconds
DT = (integer)(time unit)           "time unit" follows the GrADS convention
               integer > 0
               time unit = yr (year)
                           mo (month)
                           dy (day)
                           hr (hour)
                           mn (minute)
                           there is no seconds time unit defined by GrADS

```

One useful feature of -ndates is the format of the output date is
appropriate for the input date code and DT2. The format has the most precision for
both the input date code and DT2.

```

      Priority of the output format of the date codes (high to low)
   output date code: YYYYMMDDHHmmss      if input date code is YYYYMMDDHHmmss  DT2 cannot be in seconds
   output date code: YYYYMMDDHHmm        if input date code is YYYYMMDDHHmm   or DT2 is in minutes (mn)
   output date code: YYYYMMDDHH          if input date code is YYYYMMDDHH     or DT2 is in hours (hr)
   output date code: YYYYMMDD            if input date code is YYYYMMDD       or DT2 is in days (dy)
   output date code: YYYYMM              if input date code is YYYYMM         or DT2 is in months (mo)
   output date code: YYYY                if input date code is YYYY           or DT2 is in years (yr)

```

The -ndates was designed to provide a list of date codes which
makes scripting easier when processing time series.

```

  OLD sh                                 NEW sh
  date=2001010100                        for date in `wgrib2 /dev/null -ndates 2001010100 1mo 6hr`
  while [ $date -lt 2001020100 ]         do
  do                                        (process date)
     (process date)                      done
     date=`ndate +6 $date`
  done

#    ndate adds an hour offset to a YYYYMMDDHH date code

```

The variable precision of the output date codes makes processing by days
and months easy.

```

  for date in `wgrib2 /dev/null -ndates 20010101 1mo 1dy`
  do
     (process date)
  done

  for month in `wgrib2 /dev/null -ndates 200101 1yr 1mo`
  do
     (process month)
  done

```

By using the -ndates_fmt option, you can change the
output from a list of dates to a list of files.

```

  for file in `wgrib2 /dev/null -ndates_fmt "pgb.%s" -ndates 20010101 1mo 1dy`
  do
    echo "process $file"
  done

```

You can even make scripts.

```

  wgrib2 /dev/null -ndates_fmt "ls -l pgb.%s\n" -ndates 201801 1mo 1dy >cmd.sh

      The format contains a backslash followed by an n.

```

## Usage

```

-ndates DATE DATE2 DT2
-ndates DATE DT1 DT2

    DATE: YYYYMMDDHH or YYYYMMDDHHmm or YYYYMMDDHHmmss
    DATE2: if DT1 is given, then DATE2 = DATE + DT1

    DT1: ending dt
    DT2: step dt
    dt: (integer)Time_unit,   integer > zero
    Time_unit: yr (year)
               mo (month)
               dy (day)
               hr (hour)
               mn (minute)

    prints a list of date codes

    C-meta:    for (date=DATE; date < DATE2; date += DT2) print date;
    F-meta:    date=DATE
               while (date < DATE+DT1)
                   print date
                   date = date + DT2
               endwhile

    the output date code has the precision necessary for DATE and DT2 and
    is converted into a string.  The date is printed out using the
    "ndates_fmt" format.  The default format is " %s".  (C format)

```

### Example

```

        print date codes for 1 month, every day

$ wgrib2 /dev/null -ndates 2019020100 1mo 1dy
 2019020100 2019020200 2019020300 2019020400 2019020500 2019020600 2019020700 2019020800
2019020900 2019021000 2019021100 2019021200 2019021300 2019021400 2019021500 2019021600
2019021700 2019021800 2019021900 2019022000 2019022100 2019022200 2019022300 2019022400
2019022500 2019022600 2019022700 2019022800

$ wgrib2 /dev/null -ndates 201902 1mo 1dy
 20190201 20190202 20190203 20190204 20190205 20190206 20190207 20190208 20190209 20190210
20190211 20190212 20190213 20190214 20190215 20190216 20190217 20190218 20190219 20190220
20190221 20190222 20190223 20190224 20190225 20190226 20190227 20190228

        print date codes for 1 day, every 6 hours

$ wgrib2 /dev/null -ndates 2019020100 1dy 6hr
 2019020100 2019020106 2019020112 2019020118

$ wgrib2 /dev/null -ndates 20190201 1dy 6hr
 2019020100 2019020106 2019020112 2019020118

$ wgrib2 /dev/null -ndates 20190201 1dy 6hr
2019020100 2019020106 2019020112 2019020118

$ wgrib2 /dev/null -ndates 2019020100 1dy 6hr
 201902010000 201902010600 201902011200 201902011800

      print the months from 202001 to 202012

$ wgrib2 /dev/null -ndates 202001 202101 1mo
 202001 202002 202003 202004 202005 202006 202007 202008 202009 202010 202011 202012
$ wgrib2 /dev/null -ndates 202001 12mo 1mo
 202001 202002 202003 202004 202005 202006 202007 202008 202009 202010 202011 202012

```

### Finding the Julian date (ordinal date)

The ordinal date is the year and day of the year ranging
from 1 and 366. To get the day of the year from YYYYMMDD,
you can use -ndates.

```

wgrib2 /dev/null -ndates {YYYY-1}1231 {YYYYMMDD} 1dy | wc -w

ebis@landing2:~$ wgrib2 /dev/null -ndates 20191231 20200120 1dy | wc -w
20

```

### Limitations wgrib2 v2.8.0-v3.0.2

By default, the text output of options is stored in a buffer and written
after the option is finished. This speeds up processing because only
one large write is done rather than many smaller writes and allows you
to copy the output of inv options to other files by the -last and -last0 options.
This limited the output of -ndates to 30000 bytes (wgrib2 v2.8.0) and 100,000 bytes
(wgrib2 v3.0.0). With wgrib2 v3.0.3+, ndates no longer writes its output
to a buffer, so the output of -ndates is unlimited. This has no effect on -last and
-last0 because this options only work when processing a file, and -ndates works
prior to processing the file.

See also: [-ndates_fmt](./ndates_fmt.md),
[-ndate](./ndate.md)

---

> Description: init X Y Z X=date0 Y=(date1|dt1) Z=dt2 for (date=date0; date<(date1|date0+dt1); date+=dt2) print date

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ndates.html>_
