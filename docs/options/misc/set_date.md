# wgrib2: -set_date

## Introduction

The -set_date option changes the reference
date of the in-memory grib (sub-)message. You can write out the message
with the new date by the -grib option. If you use
-grib_out option to write out the changed grib message, you
will write the grib message with the compression method and precision that
is in effect at the time. This is slower and may lose precision.
Of course if the in-memory grid values have been modified, you have to use the
-grib_out.

The option -set_date X sets the reference time
to "date code" X, where X is
YYYY, YYYYMM, YYYYMMDD, YYYYMMDDHH, YYYYMMDDHHmm or YYYYMMDDHHmmSS.
(YYYY=year, MM=month, DD=day, HH=hour, mm=minute, SS=second)
If MM/DD/HH/mm/SS is missing, the original MM/DD/mm/SS is unaltered.

You can set the reference time to a unix time (number of seconds after
the start of January 1, 1970 by the argument "u(signed integer)".
This is convenient for converting netcdf files to grib because
netcdf files use unix time.

You can change the reference time by an offset. To change the reference time
by an offset, the argument has to start with either a negative sign or positive sign,
followed by an integer and ending with a units (yr, mo, dy, hr, or mn). Note: minutes
abreviation "mn" is easily confused as the abreviation for month and was added with wgrib2 v2.0.8.

## Usage

```

-set_date X            X = reference time, usually starting date
                       X=YYYY, YYYYMM, YYYYMMDD, YYYYMMDDHH, YYYYMMDDHHmm or YYYYMMDDHHmmSS
                       YYYY=year, MM=month, DD=day, HH=hour, mm=minute, SS=second
                       if MM/DD/HH/mm/SS is missing, the original MM/DD/mm/SS is unaltered

-set_date uN           N is a signed integer containing the number of seconds after the start
                       of January 1, 1970.  N follows the local OS convention of unix time such
                       as ignoring leep seconds.  The limitation on N is OS dependent.

                       Newer linux: N is signed 64 bit integer. However N is limited by the
                        wgrib2 implementation which stores N in a double precision variable.
                       Older linux: N is a signed 32 bit integer (problem in 2037)
                       Other OS:  varies, one OS uses N as a unsigned-32 bit integer.

-set_date -N(units)    negative offset (wgrib2 v2.0.5+)
-set_date +N(units)    positive offset (wgrib2 v2.0.5+)
                       N = unsigned integer
                       units = hr, dy, mo or yr  (GrADS time units for hour, day, month and year)
                          mn (minutes) was added with v2.0.8.

```

### Example

```

$ wgrib2 png.grb2
1:4:d=2009060500:RH:2 m above ground:330 hour fcst:ens std dev
$ wgrib2 png.grb2 -set\_date 20180101 -grib OUTFILE -s
:4:d=2018010100:RH:2 m above ground:330 hour fcst:ens std dev
$ wgrib2 png.grb2 -set\_date +12hr -grib OUTFILE -s
1:4:d=2009060512:RH:2 m above ground:330 hour fcst:ens std dev
$ wgrib2 png.grb2 -set\_date -12hr -grib OUTFILE -s
1:4:d=2009060412:RH:2 m above ground:330 hour fcst:ens std dev

Note: wgrib2 png.grb2 -set_date -12hr -grib OUTFILE -s
has a different inventory than
      wgrib2 png.grb2 -s -set_date -12hr -grib OUTFILE
because the latter prints the inventory before the date has been modified.
Note: wgrib2 png.grb2 -set_date -12hr -grib OUTFILE is the same as
      wgrib2 png.grb2 -set_date -12hr -grib OUTFILE -s
because wgrib2 will add a "-s" to the command line when it detects
that the command line has no inventory options.

```

See also:

[-grib](grib.html),
[-grib_out](grib_out.html),
[-s](s.html),
[-t](t.html),
[-T](T.html),

---

> Description: misc X changes date code, X=(+|-)N(hr|dy|mo|yr), YYYYMMDDHHmmSS, u(UNIX TIME)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_date.html>_
