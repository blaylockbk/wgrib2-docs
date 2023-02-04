
### wgrib2: -misc



### Introduction



We need names to identify objects and people. In the beginning,
there was variable names (ex. TMP, HGT), the level (ex. surface, 500 mb)
and a date code (ex. 2017010100, x seconds after 1 Jan. 1970).
Then there was a need for forecast time (ex. 12 hour forecast)
and accumulations/averages (0-6 hour accumulation). Life became
more complicated and wgrib2 used the -misc
option to print out the additional items necessary for identification.
The -misc will change with time as
more details are needed to identify the fields. As of 9/2017,
-misc includes ensemble information
(ex 10th member), probability (10% forecast), spatial processing,
wave\_partition, JMA specific, climatology, error, confidence indicator,
chemical information and aerosol information.

The -misc option is called by the 
-s option and many other similar options.


### Usage




```

-misc

```

### Example




```

$ wgrib2 percentile\_precip.grib2 -stats
1:0:75% level
2:315649:90% level
$ wgrib2 percentile\_precip.grib2 -s
1:0:d=2014101012:TPRATE:surface:2@1 hour max(13-14 hour acc fcst)++,missing=0:75% level
2:315649:d=2014101012:TPRATE:surface:2@1 hour max(13-14 hour acc fcst)++,missing=0:90% level

```


See also: [-s](./s.html)










----

>Description: inv          variable name qualifiers like chemical, ensemble, probability, etc

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/misc.html>_