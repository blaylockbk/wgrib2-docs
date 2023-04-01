# -grib_max_bits

## Introduction

When wgrib2 encodes a grib message (creates a new grib message),
the grid values are usually stored as scaled integers.
The number of bits required to store the scaled integers will depend
on the precision/scaling factors. If these values are poorly
set, the scaled integers could be 100 bits long. That would
be very inconvenient for machines with 32-bit registers.
To prevent this problem, you have to limit the size of the scaled
integers. The wgrib2 default is 16 bits, and can be increased
up to 25 bits by the -set_grib_max_bits option.
Since the IEEE single precision floating point format only has
25 bits of precision, there is little need to support longer
scaled integers at this point in time (9/2017). The
-grib_max_bits option displays the current
value of the maximum binary precision.

Grib decoders usually have a limit to the size of the scaled integers
used to store grid values. Wgrib2 has a limit of 25 bits which is determined by the minimum
size of the integer (32 bits) and the algorithm used to convert
between a bitsting and integer. I don't know the limits for
other software packages.

```
-grib_max_bits INTEGER
               INTEGER between 1 and 25
```

See also:
[-scaling](scaling.md),
[-set_grib_max_bits](set_grib_max_bits.md),

---

> Description: inv maximum bits used in grib encoding

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/grib_max_bits.html>_
