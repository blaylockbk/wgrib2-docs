# wgrib2: -set

## Introduction

Please see [new grib](./new_grib.html) for the basic
concepts of making new grib files.

The -set option changes specific metadata
of the in-memory grib (sub-)message. Expect the list of
supported fields to expand as needed. At present, the
-set option only changes fields
within the grib file. There is some overlap between various
-set\_\* options and the -set option. (Ex. -set_center N, and -set center N.)
The -set option is the newer method.

### Parameters that can be set

The available parameters that can be set will depend on the version of wgrib2 being used.
To see the parameter available,

```

$ wgrib2 grib_file -set junk junk
*** FATAL ERROR: set: allowed values: discipline, center, subcenter, master_table, local_table,
background_process_id, analysis_or_forecast_process_id, model_version_date, chemical, table_1.2,
table_1.3, table_1.4, table_3.0, table_3.1/GDT, table_3.2, table_3.3, table_3.4, table_4.0/PDT,
table_4.1, table_4.2, table_4.3, table_4.5a, table_4.5b, table_4.6, table_4.7, table_4.8,
table_4.10, table_4.11, table_4.230, table_5.0/DRT, table_6.0, % ***

```

"grib_file" has to be a grib file, and "junk" is any non-supported parameter.

## Usage

```

-set X Y             X=field, Y=integer/float/long long int
                     depending on X

                     To find the values of X, use: wgrib2 - -set help all

```

### Fields (as of wgrib2 v2.0.1)

1. discipline or table_0.0 (fixed v2.0.5)

- center
- subcenter
- master_table or table_1.0
- local_table or table_1.1
- background_process_id
- analysis_or_forecast_process_id
- model_version_date
- table_1.2
- table_1.3
- table_1.4
- table_3.0
- table_3.1 or GDT
- table_3.3
- table_3.4
- table_4.0 or PDT
- table_4.1
- table_4.2
- table_4.3
- table_4.6
- table_4.10
- table_4.11
- table_5.0 or DRT
- table_6.0

### Fields (as of wgrib2 v2.0.8)

1. most are obvious and take an integer argument

- %: integer 0..100, percentage forecast
- model_version_date: YYYYMMDDHHmmss, for PDT 4.60 and 4.61

### Example

```

$ wgrib2 -set center 99 -center
1:0:center=De Bilt, Netherlands
2:46042:center=De Bilt, Netherlands
3:63079:center=De Bilt, Netherlands
4.1:86046:center=De Bilt, Netherlands
...

```

The -set_var option will rename
all the fields in a grib file. If you only want to rename
specific fields, you will have to use the
-if and -fi options.

See also:

[-fi](fi.html),
[-grib](grib.html),
[-grib_out](grib_out.html),
[-if](if.html)
[-set_metadata](set_metadata.html)

---

> Description: misc X Y set X = Y, X=local_table,etc (help: -set help help)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set.html>_
