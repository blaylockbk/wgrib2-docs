# wgrib2: -egrep, -egrep_v, -fgrep, -fgrep_v

## Introduction

When you use wgrib2 extensively, common sequences keep occuring, such as,

```

   1:  wgrib2 A.grb >A.inv
   2:  cat A.inv | fgrep ":HGT:" | fgrep ":500 mb:" | wgrib2 -i A.grb -grib hgt500.grb
   3:  cat A.inv | fgrep ":TMP:" | fgrep ":500 mb:" | wgrib2 -i A.grb -grib tmp500.grb
   4:  cat A.inv | egrep ":(UGRD|VGRD):" | fgrep ":500 mb:" | wgrib2 -i A.grb -grib wind500.grb

```

Using the various -grep, -inv and the -i_file option, the above example can
be written as

```

   1:  wgrib2 A.grb -inv A.inv
   2:  wgrib2 -fgrep ":HGT:" -fgrep ":500 mb:" -i_file A.inv A.grb -grib hgt500.grb
   3:  wgrib2 -fgrep ":TMP:" -fgrep ":500 mb:" -i_file A.inv A.grb -grib tmp500.grb
   4:  wgrib2 -egrep ":(UGRD|VGRD):" -fgrep ":500 mb:" -i_file A.inv A.grb -grib wind500.grb

```

The first version is easier to read. So why were the extra options added?

1. Some shells have problems with pipes.
   - Some versions of Windows dos-prompt have problems with pipes.
   - RNomads: solved Windows 7 problem by using these options- More efficient when you avoid multiple processes and pipes.
   - Every millisecond and K byte of RAM usage counts!- Used by callable wgrib2.
   - A subroutine (wgrib2) can read a field using the index file!

The options were added for the third reason, but one and two are some
nice side effects. The 4 examples can be coded in fortran as,

```

   include wgrib2api
   ...
   i = wgrib2a('A.grb','-inv','A.inv')
   i = wgrib2a('-fgrep',':HGT:','-fgrep',':500 mb:','-i_file,'A.inv','A.grb','-grib','hgt500.grb')
   i = wgrib2a('-fgrep',':TMP:','-fgrep',':500 mb:','-i_file','A.inv','A.grb','-grib','tmp500.grb')
   i = wgrib2a('-egrep',':(UGRD|VGRD):','-fgrep',':500','mb:','-i_file','A.inv','A.grb','-grib','wind500.grb')

```

The -grep options are used in wgrib2api's grb2_inq(..) function.

```

Definition of grep options:

    (...) | wgrib2 -OP1 X (...)
      behaves like
    (...) | OP2 X | wgrib2 (...)

      if OP1 == egrep       then OP2 = egrep
      if OP1 == fgrep       then OP2 = fgrep
      if OP1 == egrep_v     then OP2 = egrep -v
      if OP1 == fgrep_v     then OP2 = fgrep -v

    X is a posix extended regular expression (egrep, egrep_v)
    or a fixed string (fgrep, fgrep_v)

   The number of -fgrep and -fgrep_v options is limited to 200.
   The number of -egrep and -egrep_v options is limited to 200.
   The wgrib2 option -set_regex does not affect the -grep options.

```

## Usage

```

-egrep X
-egrep_v X
-fgrep Y
-fgrep_v Y

X is a posix extended regular expression
Y is a fixed string (not a regular expression)
Note: -set_regex does not modify the type of regex for these options

```

See also:
[-match](./match.md),
[-not](./not.md),
[-match_inv](./match_inv.md),
[-end](./end.md),
[-if](./if.md),
[-i_file](./i_file.md),
[-not_if](./not_if.md),
[-set_regex](./set_regex.md).

---

> Description: init X fgrep -v X | wgrib2

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/fgrep_v.html>_
