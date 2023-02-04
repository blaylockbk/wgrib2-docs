
### wgrib2: -get\_ieee



### Introduction



The -get\_ieee option prints 
the selected parts of the grib message as an IEEE floating
point number.



### Usage




```

-get_ieee SECTION OCTET N
SECTION = section number of the grib message to print
OCTET = the octet number to print
N = number of IEEE float to print (4 octets per IEEE number)

```

### Example



```

$ wgrib2 rtgssthr\_grb\_0.083\_awips.grib2
1:0:d=2009062900:TMP:surface:anl:
  : field is surface temperature, SST over water
$ wgrib2 rtgssthr\_grb\_0.083\_awips.grib2 -packing -v
1:0:packing=grid point data - jpeg2000 compression,j val=(27133+i*2^0)*10^-2, i=0..8191 (#bits=13)
  : jpeg2000 packing, reference value is 27133
$ wgrib2 rtgssthr\_grb\_0.083\_awips.grib2 -get\_ieee 5 12 1
1:0:5-12=27133.000000
  : for jpeg2000 packing, the reference value is stored in Section 5, octet 12-15

```

See also: 
[-get\_byte](get_byte.html)
[-get\_byte](get_hex.html)
[-get\_int](get_int.html)




----

>Description: inv   X Y Z  get ieee float in Section X, Octet Y, number of floats Z

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/get_ieee.html>_