
### wgrib2: -ftn\_api\_fn0



### Introduction:



The option, -ftn\_api\_fn0, is used by the ftn\_api for
reading grib. Since the ftn\_api and wgrib2 are part of the same package, there
is no need for compatibility between versions of wgrib2. Use of this
option is not recommended. The current output is by this C statement.


```

sprintf(inv_out, "%8d %8u %8u %8u %8d %8d",inv_no,npnts,nx_,ny_,msg_no, submsg)

```





----

>Description: inv          n npnts nx ny msg_no submsg i11,5(1x,i11)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ftn_api_fn0.html>_