# wgrib2: -ext_name

## Introduction

In the beginning, grib fields were identified by a name, a level
and some timing information, like temperature, surface, 12 hour forecast
from January 1, 2021. Eventually that wasn't good enough. The field
has modifiers such as ensemble member number or median forecast.
So the concept of "extended name" was introduced into wgrib2.
So by setting a flag, some options would use the extended name
instead of the normal name (-aaig, -csv, -csv_long, -netcdf,
and macros that call -ext_name).

### Old Extended Names, up to wgrib2 3.0.2

The original extended name included the grib name and modifiers like ensemble and
tracer information (misc info). The individual items were separated by
a period, and spaces were converted into underscore. The option -set_ext_name N
was documented to accept N being 0 and 1. (The wgrib2 would accept any integer,
and would test for zero and not zero.) The option, -ext_name would print
out the extend name (name.misc-info).

### Extended Name for wgrib2 3.0.2+

Wgrib2 can write netcdf files, and -netcdf concatinates the level information
to the extended name to produce the netcdf name for the field. For some
of the newer NCEP forecast files, this name wasn't unique. So the extended
name was expanded to be name.misc-info.level-info.forecast-time.info
where the last 3 field were optional.

```
  $ wgrib2.v3.0.1 FCST.grb -set_ext_num 1 -netcdf FCST.nc
     lost fields because the field names were not unque

  $ wgrib2.v3.0.1 FCST.grb -set_ext_num 5 -netcdf FCST.nc
     extended name include misc-info and forecast-time-info.
```

The final modification to the extended name, was to make the field and space
character a run-time parameter by the option, -set_ext_name_chars. The default
values are '.' and '\_' for backwards compatibilty. The modification was needed
we now are seeing level-info and misc-info with periods in them like "0.5 mb".

The -var option prints the VARIABLE name of
the grib message. Common names would be HGT and TMP for the geopotential
height and the temperature. For most knowing the variable name,
the level and the timimg information is all you need. Then things
became more complicated. Eventually a file came along which had only
one variable type (MASSDEN, mass density) but had a couple of
important qualifier chemical type (H2O/O3/N02) and ensemble member ID.
The -AAIG output was useless because its output
used the variable name.

To fix the -AAIG output, an extended name
was introduced. You can see the extended name by
the -ext_name option.

```
$  wgrib2 chem.grb2
1:0:d=2009012600:MASSDEN:surface:anl:ENS=hi-res ctl chemical=Water Vapour
$- wgrib2 chem.grb2 -ext\_name
1:0:MASSDEN.hi-res_ctl.Water_Vapour
$ wgrib2 chem.grb2 -misc
1:0:ENS=hi-res ctl:chemical=Water Vapour
```

The extended name takes the output of -misc,
changes the colons to periods, spaces to underscores and removes the
text up to the equal size and appends it to the variable name. As of
wgrib2 v1.9.0, the extended name is used with
-AAIGc, -csv, and -netcdf.
To stop using the extended name in -AAIG, -csv and -netcdf, use the option
-set_ext_name 0.

The -ext_name prints the extended name when the extended name type
is not zero. (The -set_ext_name option sets the extended name type.)

### Extended Name when the extended name type is 0

The -ext_name always prints an extended name.
If the ext_name type is 0 which is the default value, -ext_name prints
out an extended name with the misc information. One sets the extended
name type using -set_ext_name.

## Usage

```
-set_ext_name N
       N = 0..1  wgrib2 upto v3.0.1
       N = 0..7  wgrib2 v3.0.2+
       N = 0     default value
-ext_name
       prints extended name type N if N > 0
       prints extended name type 1 if N == 0
```

Several options such as -netcdf use the
extended name type differently. If the extend name type is 0, they
use the regular name (-var).

### Examples

```
-sh-2.05b$ ./wgrib2 chem.grb2 -var
1:0:MASSDEN
-sh-2.05b$ ./wgrib2 chem.grb2 -ext\_name
1:0:MASSDEN.hi-res_ctl.Water_Vapour
```

See also: [-s](./s.md),
[-set_ext_name](./set_ext_name.md)
[-set_ext_name_chars](./set_ext_name_chars.md)

---

> Description: inv extended name, var+qualifiers

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ext_name.html>_
