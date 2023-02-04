# wgrib2: -mem_final

## Introduction

Wgrib2 supports memory files. Memory files are transient and only exist
while wgrib2 is running. Memory files
can be loaded prior to grib processing by the -mem_init option
and writen to disk after grib processing by the -mem_final option.
Memory files are rarely used in interactive wgrib2 use.

### HPC and CW2

Memory files was designed to support the reading and writing of grib files using CW2 in a HPC environment.
Suppose you want to write a grib file with a 1000 grib messages. You let 1000 CPUS encode one grib
message each. The encoding CPU may set the RPN register with the grid values.
The calling wgrib2 to encode data and write the grib2 message to a memory file.
After the call, the memory file can be read and sent to the cpus that are tasked
with writing the 1000 grib messages to disk.

## Usage

```

-mem_final N FILE
                   N=0..19
                   FILE=file to write

```

### Example

```

$ wgrib2 gep19.t00z.pgrb2af180 -mem\_final 1 OUT.grb -grib @mem:1
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:46042:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
..

```

The above line reads the file 'gep19.t00z.pgrb2af180' and and writes it
to @mem:1. At the end of the grib processing @mem:1 is written to OUT.grb.
This example shows the contents of the memory file at the end of the grib
processing.

See also: [-mem_init](./mem_init.html)

---

> Description: misc X Y write mem file X to file Y at cleanup step

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/mem_final.html>_
