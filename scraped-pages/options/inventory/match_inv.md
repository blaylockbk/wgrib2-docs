
### wgrib2: -Match\_inv



### Introduction



The -Match\_inv option is identical to 
-match\_inv except the "d=YYYYMMDDHH" and
"D=YYYYMMDDHHmmss" fields are swapped in the inventories.
The wgrib2api uses -Match\_inv option to create
its inventories.

 Wgrib2 inventories can be used as index files and as metadata
to create grib files. For index files, the -match\_inv 
makes a fine index file as it exposes a number of important parameters
that you may want to search upon. The problem with using these
files for metadata for creating grib files is that it uses a reference
time of "d=YYYYMMDDHH". Some applications require a reference time
that includes minutes (mm). The -Match\_inv option 
uses a reference time of "D=YYYYMMDDmmss" which solves this problem.


### Usage




```

-Match_inv

```


See also: 
[-match\_inv](./match_inv.html),
[-set\_metadata](./set_metadata.html),








----

>Description: inv          same as -match_inv except d=YYYYMMDDHH <-> D=YYYYMMDDHHmmss

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/Match_inv.html>_