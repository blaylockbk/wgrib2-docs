
### wgrib2: -colon



### Introduction:



The wgrib2 inventory consists of many fields separated by a colon (:). For example,


```

    $ wgrib2 small.grb2
    1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19

```


You can change the field separator from a colon to any string. For example,


```

    $ wgrib2 -colon "[--]" small.grb2
    1:0[--]d=2009060500[--]HGT[--]200 mb[--]180 hour fcst[--]ENS=+19




```

You may want to use the -colon option because
you are going to be parsing the inventory with another program that uses
a different field separator. (CSV file?).

### Usage




```

-colon string

```

### Example



```

    $ wgrib2 small.grb2 -colon ':,"' -t -colon '","' -var -lev -ftime -misc -colon '' -print '"'
    1:0:,"d=2009060500","HGT","200 mb","180 hour fcst","ENS=+19"

```


See also: 
[-print](./print.html),






----

>Description: misc  X      replace item deliminator (:) with X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/colon.html>_