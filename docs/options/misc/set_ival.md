
### wgrib2: -set\_ival



### Introduction



The -set\_ival option is used to change one or more the grid point values
of the decoded grid. After changing values, one usually writes out the grid using 
-grib\_out FILE.

### Usage




```

-set_ival I VAL
-set_ival I1:I2:..:In VAL1:VAL2:..:VALn
    I, I1, .. In is the grid point index from 1 to npnts
    VAL, VAL1, .. VALn are floating point values

```

### Example



```

$ wgrib2 small.grb2 -set\_ival 1:2:3:4 91:92:93:94 -gribout new.grb2
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
$ wgrib2 new.grb2 -csv new.csv
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
$ cat new.csv
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",0,20,91
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",10,20,92
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",0,28,93
"2009-06-05 00:00:00","2009-06-12 12:00:00","HGT","200 mb",10,28,94

```


See also: 
[-csv](./csv.html)
[-grib\_out](./grib_out.html)
[-set\_ijval](./set_ijval.html)








----

>Description: misc  X Y    sets grid point value X=i1:i2:.. Y=va1:val2:.. grid[i1] = val1,etc i>0

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_ival.html>_