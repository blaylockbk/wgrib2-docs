
### wgrib2: -set\_byte



### Introduction



The -set\_byte option sets 1 octet (byte) to a
a specified integer value. The integer value ranges from 0 to 255.



```

-set_byte I J K
  I = section number = 1..7
  J = location in the section = 1..(section length)
  K = 0 .. 255
would set 
  Section I, Octet J to the value K

```


Multiple octets can be set by making the third argument a colon seperated list.

### Usage




```

-set_byte  SECTION STARTING_OCTET_LOCATION I-1:I-2:..:I-N
SECTION=0 .. 7
STARTING_OCTET_LOCATION ≥ 1
I-M = Mth octet

```

### Example



See also: 
[-set\_hex](set_hex.html)
[-set\_ieee](set_ieee.html)
[-set\_int](set_int.html)
[-set\_int2](set_int2.html)
[-set\_ieee](set_ieee.html)










----

>Description: misc  X Y Z  set bytes in Section X, Octet Y, bytes Z (a|a:b:c)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_byte.html>_