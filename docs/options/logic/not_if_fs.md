
### wgrib2: -if\_fs, -not\_if\_fs



### Introduction



The -if\_fs option is the same as the 
the -if option except it takes "fixed strings"
rather than extended regular expressions. The same hold
for the -not\_if\_fs and -not\_if options.

### Usage




```

-if_fs X
-not_if_fs X

X is a fixed string (not a regular expression)

```


See also: [-if](./if.html), 








----

>Description: if    X      if X (fixed string) does not match, conditional execution up to next output/fi

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/not_if_fs.html>_