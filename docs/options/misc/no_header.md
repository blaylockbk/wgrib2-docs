
### wgrib2: -header, -no\_header



### Introduction



The -header and
-no\_header options
sets and clears the header flag. When the
header flag is set, binary and ieee is read and written
using f77-style header and trailers. 
When a text file (as opposed to spread sheet or csv) is written,
a preliminary line with the grid dimension, "nx ny",
is written. The default is -header.


### Usage




```

-header
-no_header

```

### Example




```

$ wgrib2 -ens ens.grb -header -ieee data.ieee
1:0:HGT:ENS=+1

```


See also: 
[-no\_header](./header.html),
[-big\_endian](./big_endian.html),
[-little\_endian](./little_endian.html),
[-ieee](./ieee.html),
[-bin](./bin.html),
[-import\_ieee](./import_ieee.html),
[-import\_bin](./import_bin.html)








----

>Description: misc         no f77 header or nx-ny header in text output

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/no_header.html>_