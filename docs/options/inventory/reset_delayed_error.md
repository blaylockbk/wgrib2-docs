# wgrib2: -reset_delayed_error

## Introduction

Wgrib2 (v3.0.1+) has two behaviors for fatal errors. The first behavior is
to quit immediately. The second behavior (delayed error) is for wgrib2 to
quit after processing the error-causing grib message. This second behavior
is useful because it allows you to examine the bad grib message, and to fix
the grib message.

The -reset_delayed_error option is an inventory
option that clears the delayed-error flags. Note that options after the
-reset_delayed_error option may also raise delayed
errors.

Processing grib files with delayed errors can cause seg faults and other
errors. For example, a delayed error can be raised when Section 3 is not of the correct
size. If wgrib2 is processing Section 3 assuming it has the right size,
and the actual size is smaller than the right size, you could have seg faults.

### Example

In this example, a delayed error causes processing to stop at the end of the message.

```

$ wgrib2 sfcu.grb

*** FATAL ERROR, local grib table=255, replaced by 1
1:0:d=2020120500:var discipline=0 center=74 local_table=1 parmcat=2 parm=192:10 m above ground:3 hour fcst:

*** FATAL ERROR (delayed): local grib table undefined (255)

```

To see the inventory without the error messages (note -s had to be added),

```

-sh-4.2$ wgrib2 sfcu.grb -s -reset_delayed_error 2>/dev/null
1:0:d=2020120500:var discipline=0 center=74 local_table=1 parmcat=2 parm=192:10 m above ground:3 hour fcst::delayed_error=4
2:3327157:d=2020120500:var discipline=0 center=74 local_table=1 parmcat=2 parm=192:10 m above ground:6 hour fcst::delayed_error=4
3:6654314:d=2020120500:var discipline=0 center=74 local_table=1 parmcat=2 parm=192:10 m above ground:9 hour fcst::delayed_error=4
4:9981471:d=2020120500:var discipline=0 center=74 local_table=1 parmcat=2 parm=192:10 m above ground:12 hour fcst::delayed_error=4

```

To change the locally define field to a standard WMO name which needs no local grib table.

```

-sh-4.2$ wgrib2 sfcu.grb -if ":var0_22_1_74_2_192:" -set_var UGRD -endif -s -reset_delayed_error 2>/dev/null
1:0:d=2020120500:UGRD:10 m above ground:3 hour fcst::delayed_error=4
2:3327157:d=2020120500:UGRD:10 m above ground:6 hour fcst::delayed_error=4
3:6654314:d=2020120500:UGRD:10 m above ground:9 hour fcst::delayed_error=4
4:9981471:d=2020120500:UGRD:10 m above ground:12 hour fcst::delayed_error=4

```

## Usage

```

-reset_delayed_error
                    delayed error flag = 0    no error
                                       > 0    see wgrib2.h for values

The delayed-error flag is the sum of the following errors (12/8/2020)

#define DELAYED_PDT_SIZE_ERR            2
#define DELAYED_LOCAL_GRIBTABLE_ERR     4
#define DELAYED_GRID_SIZE_ERR           8
#define DELAYED_FTIME_ERR               16

```

See also:
[-if_delayed_error](./if_delayed_error.html),

---

> Description: inv clear reset_delayed_error flag

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/reset_delayed_error.html>_
