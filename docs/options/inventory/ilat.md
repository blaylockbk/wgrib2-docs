
### wgrib2: -ilat



### Introduction



The -ilat option prints the latitude, longitude and 
value of ith grid point. For example, you get a text output and you
were curious about the lat-lon of the fifth grid on the list. You
would use this option to get the value and its location.

 The -ilat option uses the Fortran convention 
which has the index starting from one. In addition, the index is for the
output grid may differ from the input grid.



### Usage




```

-ilat i
      i = 1 .. number of grid points

```

### Example




```

-sh-2.05b$ wgrib2 ens.grb -ilat 5
1:0:grid pt 5,lon=4.000000,lat=-90.000000,val=18168.8
2:45932:grid pt 5,lon=4.000000,lat=-90.000000,val=8e-07
3:89724:grid pt 5,lon=4.000000,lat=-90.000000,val=2.6e-06
4:144624:grid pt 5,lon=4.000000,lat=-90.000000,val=8309.4
5:198928:grid pt 5,lon=4.000000,lat=-90.000000,val=205.9

```



See also: [-ijlat](./ijlat.html), 
[-lon](./lon.html)
[-ll2i](./ll2i.html)
[-scan](./scan.html)










----

>Description: inv   X      lat,lon and grid value at Xth grid point, X=1,..,npnts (WxText enabled)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ilat.html>_