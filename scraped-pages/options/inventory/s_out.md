
### wgrib2: -s\_out



 The -s\_out option is obsolete with the introduction of the
-last option.

```

    old:  -s_out FILE
    new:  -s -last FILE -nl_out FILE

```

### Introduction



The -s\_out option prints out a simple inventory (-s) to a file.


```

-sh-2.05b$ wgrib2 new.grb2 -s inv.dat
1:0:d=2007032600:HGT:1000 mb:anl:
2:125535:d=2007032600:HGT:1000 mb:3 hour fcst:
-sh-2.05b$ cat inv.dat
d=2007032600:HGT:1000 mb:anl:
d=2007032600:HGT:1000 mb:3 hour fcst:

```

### Usage




```

-s_out FILE

```


See also: [-s](./s.html),
[-s\_f77](./s_f77.html),
[-inv](./inv.html),








----

>Description: inv>  X      simple inventory written to X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/s_out.html>_