
### wgrib2: -print\_out



### Introduction



The -print\_out option prints a string
to a file. 



### Usage




```

-print_out "string" FILE
     Writes "string" to FILE

```

### Example



```

$ wgrib2 LIS.c3 -if ':SNOD:' \
 -s -last junk \
 -print\_out ' (found snod)' junk \
 -nl\_out junk
1:0
2:661778
3:903352
4:4108932
5:9512902:d=2014030500:SNOD:surface:anl:d=2014030500:SNOD:surface:anl:
6:14281104
7:14925511
$ cat junk
d=2014030500:SNOD:surface:anl: (found snod)

 -s -last junk                        -last prints the output from the last option (-s) to junk
 -print_out ' (found snod)' junk      writes string to junk
 -nl_out junk                         writes a new-line to junk

```



See also: 
[last](./last.html)
[last0](./last0.html)
[print](./print.html)
[nl\_out](./nl_out.html)
[s\_out](./s_out.html)




----

>Description: inv>  X Y    prints string (X) in file (Y)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/print_out.html>_