
### wgrib2: -spatial\_proc



### Introduction



Product Definition Template 4.15 is used to describe spatial processing
of a forecast or analysis. For example, you interpolate a model
grid to an output grid. For some fields you may use nearest neighbor
interpolation and for other fields you may use bilinear interpolation.
The North American Regional Reanalysis used both types of interpolation
for surface and near surface fields. Another use of PDT 15 is in
aviation. You might want the maximum clear air turbulence when
the interpolate from a fine grid to a coarse grid.




The option, -spatial\_proc, prints the
spatial properties that are encoded in PDT 4.15. This output is
part of the of the standard inventory, -s.



### Usage




```

-spatial_proc

```

### Example



```

$ wgrib2 file1 -spatial\_proc
1:0:d=2015101200:CAT:200 mb:12 hour fcst:spatial max:missing interpolation

$ wgrib2 file2 -spatial\_proc
1:0:d=2015101200:PRMSL:mean sea level:anl:spatial none:bilinear interpolation

```



See also: 







----

>Description: inv          show spacial processing, pdt=4.15

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/spatial_proc.html>_