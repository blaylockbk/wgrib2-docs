
### wgrib2: -if\_fs, -not\_if\_fs



### Introduction



The -if\_fs option is the same as the 
the -if option except it takes "fixed strings"
rather than extended regular expressions. The 
-not\_if\_fs is the same as the -not\_if option 
except it takes "fixed strings".

 The "fixed string" options have the advantage that matches do not use the
regex wildcards. For example, "-if 3.0" will match "300" because the period is
a wildcard character that matches any character including a zero. 


### Usage




```

-if_fs X
-not_if_fs X

X is a fixed string (not a regular expression)

```


See also: [-if](./if.html), 
See also: [-not\_if](./not_if.html), 
See also: [-not\_if\_fs](./not_if_fs.html), 










----

>Description: if    X      if X (fixed string), conditional execution on match

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/if_fs.html>_