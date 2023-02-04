# wgrib2: -error_final

## Introduction

The -error_final tests the final value
and can raise an error condition when wgrib2 returns.
For version 1, the only value that can be tested is the count
of the number time the option is called in the grib-processing phase.

When -error_final is intialized, the count is set to zero.
Then -error_final increments the count whenever it processes
a grib message.
Finally when -error_final is run after processing all the grib
messages, it tests count to a specified integer and sets the error return.

## Usage

```

-error_final count TEST INTEGER
   TEST = eq, ne, lt, gt, le, ge
   INTEGER integer
   sets return error to one if "count TEST INTEGER" is true

```

### Example

```

-sh-4.2$ wgrib2 LIS.c3
1:0:d=2014030500:SPRATE:surface:anl:
2:661778:d=2014030500:RPRATE:surface:anl:
3:903352:d=2014030500:SKINT:surface:anl:
4:4108932:d=2014030500:SDWE:surface:anl:
5:9512902:d=2014030500:SNOD:surface:anl:
6:14281104:d=2014030500:PRATE:surface:anl:
7:14925511:d=2014030500:TMP:surface:anl:
-sh-4.2$ wgrib2 LIS.c3  -error_final count ne 7 -if ':TMP:' -error_final count ne 1 -endif
1:0:d=2014030500:SPRATE:surface:anl:
2:661778:d=2014030500:RPRATE:surface:anl:
3:903352:d=2014030500:SKINT:surface:anl:
4:4108932:d=2014030500:SDWE:surface:anl:
5:9512902:d=2014030500:SNOD:surface:anl:
6:14281104:d=2014030500:PRATE:surface:anl:
7:14925511:d=2014030500:TMP:surface:anl:
-sh-4.2$ echo $?
0
-sh-4.2$ wgrib2 LIS.c3  -error_final count ne 7 -if ':TMP:' -error_final count ne 3 -endif
1:0:d=2014030500:SPRATE:surface:anl:
2:661778:d=2014030500:RPRATE:surface:anl:
3:903352:d=2014030500:SKINT:surface:anl:
4:4108932:d=2014030500:SDWE:surface:anl:
5:9512902:d=2014030500:SNOD:surface:anl:
6:14281104:d=2014030500:PRATE:surface:anl:
7:14925511:d=2014030500:TMP:surface:anl:
-sh-4.2$ echo $?
1

```

See also:
[-if](./if.md)
[-endif](./endif.md)

---

> Description: misc X Y Z error if at end X=count Y=ne,eq,le,lt,gt,ge Z=integer

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/error_final.html>_
