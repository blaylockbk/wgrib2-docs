
### wgrib2: -if\_delayed\_error



### Introduction



Wgrib2 v3.0.1 introduces delayed errors. The (-if\_delayed\_error) allows you
to check the delayed error flag and perhaps run a process to fixed the delayed errors.
Unlike most -if options, there isn't an equivalent
-elseif\_delayed\_error option.

If you are interested in writting an "if" option for wgrib2, the source code, If\_delayed\_error.c, 
is an ideal example.

### Example



fields with delayed errors will have the full inventory printed

$ wgrib2 bad\_n\_header.grb2 -if\_delayed\_error -s -endif -reset\_delayed\_error
1:0:delayed\_error=0
2:154212:delayed\_error=0
3:308556:delayed\_error=0
4:311920:delayed\_error=0
5:312107:delayed\_error=0
6:324894:delayed\_error=0
7:325081:delayed\_error=0
8:330180:delayed\_error=0
9:345421:delayed\_error=0
10:401570:delayed\_error=0
11:401757:delayed\_error=0
12:401944:delayed\_error=0
13:477882:delayed\_error=0
14:615677:delayed\_error=0
15:803947:delayed\_error=0
16:899104:delayed\_error=0
17:1041002:delayed\_error=0
18:1190330:delayed\_error=0
19:1194054:delayed\_error=0
\*\* ERROR bad grib message: Statistical Processing bad n=0 \*\*
20:1194241:d=2017041700:LRGHR:1 hybrid level:::delayed\_error=18
21:1229461:delayed\_error=0
22:1383477:delayed\_error=0
23:1383916:delayed\_error=0
24:1384103:delayed\_error=0
..

### Usage




```

-if_delayed_error   will enter if block if there is a delayed error

```


See also:
 [-reset\_delayed\_error](./reset_delayed_error.html), 










----

>Description: if           if delayed error

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/if_delayed_error.html>_