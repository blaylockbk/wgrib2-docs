
### wgrib2: -set\_int2



### Introduction



The -set\_int2 option sets 2 octets to a
signed integer value which is commonly used by grib.
The integer must range from -32767 to 32767. 



```

-set_int2 I J K
  I = 1..7
  J = 1..(section length-1)
  K = -(2**15-1) .. (2**15-1)
would set 
  Section I, Octet J+0:  if (K >= 0) (K >> 8) & 255
  Section I, Octet J+0:  if (K < 0) ((abs(K) >> 8 ) && 255) | 128
  Section I, Octet J+3:   abs(K) & 255;
 The above is using C syntax.

```


Multiple integers can be set by making the third argument a colon seperated list.

### Usage




```

-set_int2  SECTION STARTING_OCTET_LOCATION I-1:I-2:..:I-N
SECTION=0 .. 7
OCTET_LOCATION = 1..N
I-M = Mth integer

```

### Example



See also: 
[-get\_int](get_int.html)
[-set\_byte](set_byte.html)
[-set\_hex](set_hex.html)
[-set\_ieee](set_ieee.html)
[-set\_int](set_int.html)










----

>Description: misc  X Y Z  set 2-byte ints in Section X, Octet Y, signed integers Z (a|a:b:c)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_int2.html>_