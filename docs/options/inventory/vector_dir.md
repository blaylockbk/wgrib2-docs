
### wgrib2: -vector\_dir



### Introduction



Bit 5 of the flag 3.3 indicates whether vector quantities are relative to the
grid or the North/South poles. 
The -vector\_dir option writes out "winds(N/S)" or "winds(grids)"
depending on the value of the flag. Note there is no flag that indicates whether
the quantity is a U/V component of a vector.


### Usage




```

-vector_dir

```

### Example




```

$wgrib2 png.grb2 -vector\_dir
1:4:winds(N/S)

```


See also: 










----

>Description: inv          grid or earth relative winds

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/vector_dir.html>_