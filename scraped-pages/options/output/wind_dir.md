
### wgrib2: -wind\_dir



### Introduction



The -wind\_dir option takes the zonal and meridional winds,
computes the wind direction and writes it out in grib format. The results
is in degrees where 0 degrees is a wind from the north and 90 degrees is a wind from
the east. For this option to 
work, (1) the zonal and meridional winds must be earth relative (not grid relative
winds) and (2) the meridional wind must follow the corresponding zonal wind for that
level, time and forecast hour. (Condition 2 is the same restriction as for the
-wind\_speed option. If the U and V in your grib file do not have
this order, the file must be sorted. However, many NCEP forecast files already
have this order.


The winds need to be earth relative. The -new\_grid option 
can convert between earth and grid relative winds.


You can save computer time by using the -match option 
to restricting the decoding of records to U and V.

 The precision of the wind direction is set to the nearest degree (wgrib2 v3.0.0). 
Earlier versions used the precision entering the routine. That would be precision
of the V field unless the precision was changed by a scaling option like -set\_scaling.

### Calm Winds


 The calculation of the wind direction breaks down when the zonal and meridional
winds are zero. The wind direction for calm winds depends on the implementation.
(C's atan2 is well defined but some machines do not implement negative zero.)


### Usage




```

-wind_dir output_grib_file

   Comment: the -wind_speed option can write to the same file as -wind_dir

```

### Example




```

$ wgrib2 gep19.t00z.pgrb2af180 -wind\_dir wind.grb -wind\_speed wind.grb -match "(UGRD|VGRD)"
4.1:86046:d=2009060500:UGRD:200 mb:180 hour fcst:ENS=+19
4.2:86046:d=2009060500:VGRD:200 mb:180 hour fcst:ENS=+19
8.1:226831:d=2009060500:UGRD:250 mb:180 hour fcst:ENS=+19
8.2:226831:d=2009060500:VGRD:250 mb:180 hour fcst:ENS=+19

$ wgrib2 wind.grb
1:0:d=2009060500:WDIR:200 mb:180 hour fcst:ENS=+19
2:97922:d=2009060500:WIND:200 mb:180 hour fcst:ENS=+19
3:179554:d=2009060500:WDIR:250 mb:180 hour fcst:ENS=+19
4:277476:d=2009060500:WIND:250 mb:180 hour fcst:ENS=+19

```


See also: [-match](./match.html), 
 [-wind\_speed](./wind_speed.html)
[-wind\_uv](./wind_uv.html)


















----

>Description: out   X      calculate wind direction, X = output gribfile (direction in degrees, 0=wind from north, 90=wind from east)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/wind_dir.html>_