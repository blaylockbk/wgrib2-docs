
### wgrib2: -elseif\_fs



### Introduction



The -elseif\_fs option is part of the 
 -if, 
 -else, 
 -endif structure for conditional execution of wgrib2 options.
The -elseif\* option must follow an
 -if or -elseif type option, and it must preceed either the next
 -elseif type,
 -else or 
 -endif option.


The -elseif\_fs is an
-elseif\* options and
does a comparison with a fixed string (fs) instead
of a regular expression as done by the -else\_if.

More details are in the [-if](./if.html) documentation.
The -else option was introduced with wgrib2 v3.0.0 to replace
the older version 1 IF structure.


See also: 
[-if](./if.html), 
[-if\_fs](./if_fs.html), 
[-else](./else.html), 
[-elseif](./elseif.html), 
[-endif](./endif.html), 
[-match](./match.html),










----

>Description: elif  X      elseif X (fixed string) conditional execution

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/elseif_fs.html>_