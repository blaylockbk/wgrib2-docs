
### wgrib2: -mem\_del CW2



### Introduction



Wgrib2 supports memory files. Memory files are transient and only exist 
while wgrib2 is running. Memory files
can be loaded prior to grib processing by the -mem\_init option,
writen to disk after grib processing by the -mem\_final option
and deleted during the processing phase by the -mem\_del option.
Memory files can be used with the wgrib2 utility but were designed for use by callable wgrib2 (CW2).


### HPC and CW2



Memory files was designed to support the read and writing of grib files using CW2 in
a HPC environment. Suppose you want to read a grib file with a 1000 grib messages.
One CPU reads the grib file, divides it into 1000 grib messages and sends one
grib message to 1000 different CPUS. Each of the CPUS is reponsible for
reading 1 grib message. Each CPU receives a message (memory buffer) with
a grib message and needs to decode that data. Rather than writing that
grib message to disk, the CPU will write the grib message to memory file
and then have wgrib2 decode the memory file, saving the metadata to another
memory file and the grid data to a rpn register. The CPU can then get the
metadata and grid from memory. 



### Usage



```

-mem_del N
                   N=0..19

```

### Example



The -mem\_del option is intended for the use by callable wgrib2
so that memory files can be deleted and the memory freed.


See also: [-mem\_final](./mem_final.html),
[-mem\_init](./mem_init.html),








----

>Description: misc  X      delete mem file X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/mem_del.html>_