# wgrib2: -set_grib_max_bits

## Introduction

With most grib packing methods, the grid values are
usually stored as scaled integers. The grib format allows
the scaled integers to have up to 254 bits (simple packing).
This is an unreasonably high precision as wgrib2 converts
the data to a single-precision floating point number which
typically has only 25 bits of precision. To speed up
the encoding and decoding processing, wgrib2 uses 32 bit
integers which limits the maximum size of scaled integers
to 25 after accounting for the limits of the packing and
unpacking routines.

Grib fields are usually stored as a scaled integers,
and usually you don't need 25 bits of precision. For
example, temperature to the nearest 0.1 degree, means
that the a temperature range of 50 C can be stored as
500 descrete values and only 7 bits are needed to store
the temperature to the nearest 0.1 degree.

The problem is that people can set the scaling factors
to inappropriate values, using 25 bits of precision will
was (disk) space. Consequently wgrib2 has a resetable limit
of the precision of the scaled integers allowed. If you
exceed the limit, the scaling factors are change so
that the scaled integers are limited.

The -set_grib_max_bits option sets the maximum
number of bits that the scaled integers can have when
encoding data into grib. The value
should never be greater than 25 as that is the limit for wgrib2 encoding.
However, some grib packages may not support 25 bit precision in decoding,
so you may want to limit the precision to 24 to remain compatible with
other software packages.

The -set_grib_max_bits option does not set the
binary precision of the grib output but sets the maximum possible precision
of the numbers.

## Usage

```

-set_grib_max_bits N
  N = maximum number of bits used to encode data
  the default value is 16, N = 1..25

```

### Comments

The -set_grib_max_bits option does not affect
grib data written using the -grib option because the
-grib option does not encode data but copies
the input grib message.

See alse:
[-set_scaling](set_scaling.md)
[-set_bin_prec](set_bin_prec.md)
[-grib_max_bits](grib_max_bits.md)

---

> Description: misc X sets scaling so number of bits does not exceed N in (new) grib output

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_grib_max_bits.html>_
