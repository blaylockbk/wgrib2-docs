
### wgrib2: -precision



### Precision



Except for grid values stored in IEEE format, grid values are
stored with user-defined precision. By having a variable precision,
the space required can be reduced. For having the temperature 
stored to a thousandth of a degree would be wasteful as thermometers
typically give a result to a tenth of a degree.

 Grid point values are stored as scaled integers, the scaling
determines the precision.


```

val = (base+i*2^bin_scaling)*10^dec_scaling
       base = real*4
       bin_scaling = integer from -127 .. 127
       dec_scaling = integer from -127 .. 127

```

 The -precision option prints the bin\_scaling
and dec\_scaling values.
The -precision output can be read by
-set\_metadata and
-set\_metadata\_str options to set the precision.


### Usage




```

-precision

```

### Examples



```

$ wgrib2 ecmwf.tp.hc -precision
1:0:encode i*2^-4*10^0

$ wgrib2 wind.grb -precision
1:0:encode i*2^0*10^-1
2:97922:encode i*2^0*10^-1
3:179554:encode i*2^0*10^-1

```


See also: 
[-packing](./packing.html),
[-scale](./scale.html),
[-set\_bin\_prec](./set_bin_prec.html),
[-set\_metadata](./set_metadata.html),
[-set\_metadata\_str](./set_metadata_str.html),
[-set\_scale](./set_scale.html),












----

>Description: inv          precision of packing

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/precision.html>_