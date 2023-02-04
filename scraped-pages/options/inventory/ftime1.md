
### wgrib2: -ftime1



### Introduction



The -ftime1 option calls the old ftime1 routine.
The ftime1 routines have been replace by the ftime2 routine because they
could not handle more complicated time stamps correctly. One should
not use -ftime1. The ftime2 routines will work with
more complicated time stamps and are more powerful.

### Usage




```

-ftime1

```

### Example




```

$ wgrib2 grib2.polar -ftime1
1.1:0:24 hour fcst
1.2:0:24 hour fcst

```



See also: 
[-ftime](./ftime.html)








----

>Description: inv          forecast time

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ftime1.html>_