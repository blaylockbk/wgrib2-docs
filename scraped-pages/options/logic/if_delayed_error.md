# wgrib2: -if_delayed_error

## Introduction

Wgrib2 v3.0.1 introduces delayed errors. The (-if_delayed_error) allows you
to check the delayed error flag and perhaps run a process to fixed the delayed errors.
Unlike most -if options, there isn't an equivalent
-elseif_delayed_error option.

If you are interested in writting an "if" option for wgrib2, the source code, If_delayed_error.c,
is an ideal example.

### Example

fields with delayed errors will have the full inventory printed

$ wgrib2 bad_n_header.grb2 -if_delayed_error -s -endif -reset_delayed_error
1:0:delayed_error=0
2:154212:delayed_error=0
3:308556:delayed_error=0
4:311920:delayed_error=0
5:312107:delayed_error=0
6:324894:delayed_error=0
7:325081:delayed_error=0
8:330180:delayed_error=0
9:345421:delayed_error=0
10:401570:delayed_error=0
11:401757:delayed_error=0
12:401944:delayed_error=0
13:477882:delayed_error=0
14:615677:delayed_error=0
15:803947:delayed_error=0
16:899104:delayed_error=0
17:1041002:delayed_error=0
18:1190330:delayed_error=0
19:1194054:delayed_error=0
\*\* ERROR bad grib message: Statistical Processing bad n=0 \*\*
20:1194241:d=2017041700:LRGHR:1 hybrid level:::delayed_error=18
21:1229461:delayed_error=0
22:1383477:delayed_error=0
23:1383916:delayed_error=0
24:1384103:delayed_error=0
..

## Usage

```

-if_delayed_error   will enter if block if there is a delayed error

```

See also:
[-reset_delayed_error](./reset_delayed_error.md),

---

> Description: if if delayed error

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/if_delayed_error.html>_
