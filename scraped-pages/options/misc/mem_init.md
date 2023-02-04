# wgrib2: -mem_init CW2

## Introduction

Wgrib2 supports memory files. Memory files are transient and only exist
while wgrib2 is running. Memory files
can be loaded prior to grib processing by the -mem_init option
and writen to disk after grib processing by the -mem_final option.
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

## Usage

```

-mem_init N FILE
                   N=0..19
                   FILE=file to read

```

### Example

```

$ wgrib2 -mem\_init 10 gep19.t00z.pgrb2af180 @mem:10
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:46042:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
..

```

The above line reads the file 'gep19.t00z.pgrb2af180' and save it in
memory file, @mem:10, before grib processing. Wgrib2 processes
the memory file, @mem:10. This example has no practical application;
however, the following can be used.

```

$ wgrib2 IN.grb | sort -t: -k3,7 | wgrib2 -i -mem\_init IN.grb 0 @mem:0 -grib OUT.grb

```

The above line takes the original file, IN.grb, and writes it out in sorted order. By using
a memory file, a random access read is replaced by the much faster sequential read.

See also: [-mem_final](./mem_final.md),
[-rpn](./rpn.md)

---

> Description: misc X Y read mem file X from file Y (on initialization)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/mem_init.html>_
