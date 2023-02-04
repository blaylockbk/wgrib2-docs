
### wgrib2: -gdt



### Introduction



Section 3 contains the grid definition template (GDT) 
and the -gdt option prints the
parameters used to define the grid as
used by g2clib/g2lib (NCEP libraries). This option is
only useful finding the grid parameters for later use
by g2clib/g2lib. 




The -gdt option will only
work if g2clib is installed at compile time.



### Usage




```

-gdt

```

### Example



```

$ wgrib2 -gdt png.grb2
1:4:GDT Number= 0 GDT= 6 0 0 0 0 0 0 360 181 0 0 90000000 0 48 -90000000 359000000 1000000 1000000 0

```



See also: 





----

>Description: inv          contents of Grid Definition Template (g2c)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/gdt.html>_