
### wgrib2: -set\_metadata\_str



### Introduction



The -set\_metadata\_str "string" option is similar to the
older -set\_metadata FILE option. Instead of reading
the metadata from a file, the metadata is on the command line. The latter
option is generally more useful as each grib message can have its own
set of metadata. The former option is only useful for specifying the metadata
for a single grib message. The -set\_metadata\_str option 
was added to facilitate the creation of an "callable wgrib2" API for writing grib2.
See [-set\_metadata](./set_metadata.html) for the format of the metadata string.


### Usage:



```

-set_metadata_str "metadata"
metadata is a string 

```

### Example:



```

sh-4.1$ wgrib2 small.grb2 -set\_metadata\_str "1:0:d=2001020304:TMP:10 mb:anl:"
1:0:d=2001020304:TMP:10 mb:anl:ENS=+19  


```


See also:
[-set\_metadata](./set_metadata.html),







----

>Description: misc  X      X = metadata string

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_metadata_str.html>_