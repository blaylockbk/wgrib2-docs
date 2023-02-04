# wgrib2: pipes and named pipes (fifo), POSIX only

## Introduction

This web page applies to POSIX compliant operating systems.
Linux and Unix are either fully or mostly POSIX-compliant.
Windows may or may not be POSIX compliant. To quote the POSIX Wiki,
"The UNIX Subsystem is built in to the Enterprise and Ultimate
editions of Windows Vista and 7, and cannot be added separately
to the other editions." Third-party add-ons may or may not provide
POSIX pipe support. Windows provides support for unnamed pipes, "|".
However, using these pipes for binary data will be compiler and
perhaps OS dependent. Note: prior to the 5/2011 version of wgrib2, the
-flush option was needed in the following examples.

Pipes are used in the old-style selection of fields. In the
following example, the first wgrib2 creates and inventory, the grep
selects the HGT field and the second wgrib extracts the HGT based
on the input inventory.

```

  wgrib2 IN.grb | grep ":HGT:" | wgrib2 IN.grb -i -grib OUT.grb


```

A more modern example of pipes is to extract all the HGT fields
from a set of files. In the following example, the "cat" writes
all the 2016 grib files to the pipe. The wgrib2 reads the pipe (file=-)
and writes all the HGT fields to the file HGT2016.grb.

```

  cat pgb.2016????? | wgrib2 - -match ":HGT:" -grib HGT2016.grb


```

Pipe are great because this they avoid the slow disk I/O and allow
two processes to run at the same time. For example, the above
line could be executed as,

```

  cat pgb.2016????? tmpfile

  wgrib2 tmpfile -match ":HGT:" -grib HGT2016.grb

  rm tmpfile


```

An example where two process run at the same time is
given below. The first wgrib2 regrids the fields and writes it
to stdio. The second wgrib2 reads the regridded fields and
writes a CSV file. On a multi-core system, both processes
and be computing at the same time.

```

  wgrib2 IN.grb -inv processed.txt  -new_grid_winds earth -new_grid ncep grid 2  - | wgrib2 - -csv out.csv

```

Another usage of pipes is to avoid the 2GB limit on input grib2 file when using a 32-bit machine.

```

 cat BIGFILE | wgrib2 - -match ':HGT:' -grib HGT.grb

```

### Named Pipes

Named pipe are also useful. Consider the following line
which converts the file into jpeg2000 packing.

```

   wgrib2 IN.grb -set_grib_type jpeg -grib_out JPEG.grb

```

Jpeg2000 packing is slow and you can convert the file using
3 cpus and named pipes.

```

1  mkfifo pipe.1.$$ pipe.2.$$ pipe.3.$$
2  wgrib2 IN.grb -for_n 1::3 -set_grib_type jpeg -grib_out pipe.1.$$ &
3  wgrib2 IN.grb -for_n 2::3 -set_grib_type jpeg -grib_out pipe.2.$$ &
4  wgrib2 IN.grb -for_n 3::3 -set_grib_type jpeg -grib_out pipe.3.$$ &
5  gmerge JPEG.grb pipe.1.$$ pipe.2.$$ pipe.3.$$
6  rm pipe.1.$$ pipe.2.$$ pipe.3.$$

  line 1: makes the named pipes
  line 2: wgrib2 command process fields 1,4,7.., writes to pipe.1.$$
  line 3: wgrib2 command process fields 2,5,8.., writes to pipe.2.$$
  line 4: wgrib2 command process fields 3,6,9.., writes to pipe.3.$$
  line 5: gmerge copies to JPEG.grb grib messeages from pipe.1,
          pipe.2 and then pipe.3, and starts at pipe.1 again.
          This round robin selection preserves the order of fields.
  line 6: cleanup

  * versions of wgrib2 prior to 5/2011 needed the -flush option for lines 2-4.

```

The above is a simple example of multitasking a wgrib2 job.

|

|     |
| --- |

|

---

|
| [NOAA/](https://www.noaa.gov/)
[National Weather Service](https://www.nws.noaa.gov/)
[National Centers for Environmental Prediction](https://www.ncep.noaa.gov/)
Climate Prediction Center
5830 University Research Court
College Park, Maryland 20740
[Climate Prediction Center Web Team](/comment-form.html)
Page last modified: Oct 18, 2017
| [Disclaimer](https://weather.gov/disclaimer.php) | [Privacy Policy](https://weather.gov/privacy.php) |

|

---

> Description: pipes and named pipes, POSIX only

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pipes.html>_
