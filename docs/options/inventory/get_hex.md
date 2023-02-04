
### wgrib2: -get\_hex



### Introduction



The -get\_hex option is identical to the
-get\_byte option excepts it prints the
bytes in hexidecimal format.
see the 20 and 21 octet (byte in WMO speak) of section 4, you would
use -get\_byte 4 20 2. The first argument
is the section number. The second is the byte (octet) number starting
from 1 (consistent with WMO documentation) and the third is the number
of octets to display. 




The input arguments to the option are in decimal rather than in hexidecimal because
all the grib2 documentation uses decimal numbers to specify the byte locations.



### Usage




```

-get_hex SECTION OCTET NUMBER
SECTION = section to print
OCTET = starting octet to print
NUMBER = number of octets/bytes to print

```

### Example



```

$ wgrib2 f.grb2 -get\_hex 0 1 12
1:0:0-1=71,52,49,42,00,00,00,02,00,00,00,00
2:46042:0-1=71,52,49,42,00,00,00,02,00,00,00,00
3:63079:0-1=71,52,49,42,00,00,00,02,00,00,00,00

```



See also: 
[new grib](new_grib.html),
[-set\_byte](set_byte.html)
[-get\_ieee](get_ieee.html)
[-get\_int](get_int.html)




----

>Description: inv   X Y Z  get bytes in Section X, Octet Y, number of bytes Z (bytes in hexadecimal format)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/get_hex.html>_