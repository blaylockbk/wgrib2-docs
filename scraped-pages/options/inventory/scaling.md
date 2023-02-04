# wgrib2: -scaling

## Introduction

In grib, grid values are usually stored as

```

  grid_value = (ref_value + I*2**bin_scaling)) * 10**decimal_scaling.
    I is an positive integer
    ref_value = 32-bit IEEE floating point number
    binary_scaling = -127..127
    decimal_scaling = -127..127
    number_of_bits = the smallest N such that I < 2**N
      note: WMO does not place a maximum value of the number of bits
            however, implementations do have limits.  The limits will
            vary by software package and procedure.

```

This is not an absolute as the grid point values can be stored as
a spectral coefficients, IEEE floating point values and other formats.
The -scaling option
prints the binary and scaling factors, the reference value and the number
of bits used for the integer I.

## Usage

```

-scaling

```

### Example

```

$ wgrib2 gep19.t00z.pgrb2af180 -scaling -for 13:14 -v -packing
13:473374:scaling ref=2.32109e+06 dec_scale=-3 bin_scale=4 nbits=16:packing=grid point data - jpeg2000 compression,j val=(2.32109e+06+i*2^4)*10^-3, i=0..65535 (#bits=16)
14:528630:scaling ref=2215 dec_scale=-1 bin_scale=0 nbits=10:packing=grid point data - jpeg2000 compression,j val=(2215+i*2^0)*10^-1, i=0..1023 (#bits=10)

```

See also:
[-scale](./scale.html)
[-set_scaling](./set_scaling.html)
[-grib_max_bits](./grib_max_bits.html)

---

> Description: inv scaling for packing (old format)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/scaling.html>_
