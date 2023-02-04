
### wgrib2: -GRIB



### Introduction



The -GRIB option is used to copy an entire
GRIB2 message (including all the submessages) to a specified file.
This is useful if you want to preserve to keep the submessages.
This is in contrast to the -grib option which writes
submessages into its own grib message. 

 The -GRIB option should only be used
to copy the original grib message to another file. Unlike the
the -grib option, changing the metadata
is not recommended. 

### Usage




```

-GRIB file_name

```


See also: [-grib](./grib.html)
[-ncep\_uv](./ncep_uv.html)
[-submsg](./submsg.html)
[-tosubmsg](./tosubmsg.html)
[-append](./append.html)








----

>Description: out   X      writes entire GRIB record (all submessages)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/GRIB.html>_