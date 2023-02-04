# wgrib2: SPEED

## Introduction

Here are some techniques to make wgrib2 faster.

### Stop using JPEG2000 and PNG compression

Complex packing (c1, c2 and c3) are much faster. For fields with undefined
grid points, complex packing produces smaller files. For global fields, the
optimum complex packing is typically 20% larger than jpeg2000 compression.
Converting is easy,

```

    wgrib2 IN.grb -set_grib_type c2 -grib_out OUT.grb
      on a 4 core CPU, this is very fast
    wgrib2ms 4 IN.grb -set_grib_type c2 -grib_out OUT.grb

```

When you make complex packed files, do not uses a bitmap to store
undefined values, use the special-value method. If you use special
value method, the resulting files will be smaller and the decoder
is very parallelized. Using bitmaps with complex packed files
will result in larger files that are slightly slower to decode.
Wgrib2 can make both flavors of complex-packed files. The comments
about parallelization apply to the wgrib2-internal decoders.

The best complex packing (c1, c2, c3) depends on the field.
C3 is best for smooth fields and c1 is best for more noisy fields
like precipitation. Once in a while, c2 produces the smallest fields.
Multitasking scripts that convert the grib file int c1, c2 and c3 packing
and then choses the smallest grib message.

AEC compression is much faster than jpeg2000 and png compression. Before
you use AEC, make sure that any of your essential grib software supports
this brand new compression (9/2016). Converting is easy,

```

    wgrib2 IN.grb -set_grib_type aec -grib_out OUT.grb

```

### Minimize the decoding

Decoding the grid point values is an expensive operation especially for jpeb2000 compressed files.
It is useful to count the number of decodes,

```

Decodes all records:   wgrib2 IN.grb -if "HGT:200 mb:" -csv csv.txt
Decodes one record:    wgrib2 IN.grb -match "HGT:200 mb:" -csv csv.txt

Decodes 3M records:    wgrib2 IN.grb -lon 0 10 >lon1.txt
                       wgrib2 IN.grb -lon 0 20 >lon2.txt
                       wgrib2 IN.grb -lon 0 30 >lon2.txt
Decodes  M records:    wgrib2 IN.grb -lon 0 10 -lon 0 20 -lon 0 20  >lons.txt

note: the number of locations is limited by the number of command line
arguments (5000 v1.9.8).  Other limitations include the number of
allowed regular expressions (1000 v1.9.8) and any system limitations
on the length of the command line.  (Cygwin 32K, XP=8K linux=varies
but 2.5MB on RHEL 6)

```

### Don't use the wgrib2 | egrep | wgrib2 -i syntax

The "wgrib2 | egrep | wgrib2" syntax can be optimized.

```

M sequential reads + N random-access reads, N writes
     wgrib2 file | grep ":TMP:" | wgrib2 -i file -grib tmp.grb

M sequential reads, N writes
     wgrib2 file -match ":TMP:" -grib tmp.grb

```

### Making an inventory/index file

If you process the same grib files many times, an inventory/index file is faster.

```

3M reads, N writes
     wgrib2 file -match ":TMP:" -grib tmp.grb
     wgrib2 file -match ":HGT:" -grib hgt.grb
     wgrib2 file -match ":(UGRD|VGRD):" -grib winds.grb

M+N reads, N writes + i/o for file.inv
     wgrib2 file >file.inv
     grep <file.inv ":TMP:" | wgrib2 -i file -grib tmp.grb
     grep <file.inv ":HGT:" | wgrib2 -i file -grib hgt.grb
     grep <file.inv ":(UGRD|VGRD):" | wgrib2 -i file -grib winds.grb


```

### Do alot on one line

The above example can be made faster by doing it all on one command line.

```

M reads, N writes
     wgrib2 file \
         -if ":TMP:" -grib tmp.grb \
         -if ":HGT:" -grib hgt.grb \
         -if ":(UGRD|VGRD):" -grib winds.grb

```

### Use -bin instead of -ieee

-bin writes/reads the grid values using the native floating point format.

-ieee writes/reads the grid values using the ieee format (big-endian is default).

-ieee uses software to convert from the native format to ieee even when the native
format is ieee.

### Fast Averaging

If you are averaging, use
[fast averaging](https://www.cpc.noaa.gov/products/wesley/wgrib2/ave.html)

### Compile wgrib2 with OpenMP Enabled

More cores are better for large grids. For large grids, use complex packing
without bitmaps. The decoder is parallelized for this case and runs 3x faster
for 3 km global grids on a 6-core machine.

### Run wgrib2 without OpenMP Enabled for Small Grids

For small grids, OpenMP can slow the program. You can compile
with OpenMP turned off or with "OMP_NUM_THREADS" set to one.

### Use multple cores

See [here](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pipes.html)
and over [there](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/for_n.html)

### Use pipes instead of temporary files

Wgrib2 can read and write through pipes. This is both a programming convenience
as well as faster.

```

cat gfs*201201*grb2 | wgrib2 - -match ":HGT:500 mb:" -grib Z500.grb

```

---

> Description: make wgrib2 faster (speed)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/speed.html>_
