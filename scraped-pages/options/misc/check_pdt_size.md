
### wgrib2: -check\_pdt\_size



### Introduction



Wgrib2 v3.0.1 will check the size of the Product Definition Template (PDT, Section 4).
If the PDT is wrong size, wgrib2 will exit with a delayed fatal error. The fatal
error is delayed so the user can investigate the contents of the bad grib message.
Note that it is possible to seg fault because Section 4 is a different size than
expected, and the rest of the grib message may be corrupted.


You may skip the pdt size check using the option -check\_pdt\_size 0.
You would use this option if wgrib2 calculates the incorrect size of Section 4.

### Usage




```

-check_pdt_size    0 disable PDT size check
                   1 enable  PDT size check

```

### Example




```

$ wgrib2 png.grb2 -check\_pdt\_size 0
1:4:YY=2009

```




See also: 










----

>Description: misc  X      check pdt size X=1 enable/default, X=0 disable

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/check_pdt_size.html>_