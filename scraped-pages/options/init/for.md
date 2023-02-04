
### wgrib2: -for



### Introduction



The -for option selects the range of record numbers 
upon which to operate using the "for" syntax. If you want to operate on 
records 10 to 20, you would use the parameter 10:20. 
If you want to operate on all the even records from 10 to 20, you would 
use 10:20:2. The restrictions are the start value must be less than 
the ending value and the step has to be greater than zero.




The -for option ignores the submessage number when
selecting the fields.



### Usage




```

-for I:J:K        same as for n = I to J by K
-for I:J          same as for n = I to J by 1
-for I::K         same as for n = I to MAX_INTEGER by K
-for I            same as for n = I to MAX_INTEGER by 1

```

### Example




```

 $ wgrib2 file.grb2 -for 4:5
4:13335:d=2008120200:RH:750 mb:anl:
5:17098:d=2008120200:TMP:2743 m above mean sea level:anl:

```


See also: 
[-if\_rec](./if_rec.html)
[-match](./match.html)
[-for\_n](./for_n.html)






----

>Description: init  X      process record numbers in range, X=(start:end:step), only one -for allowed

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/for.html>_