# wgrib2: -scale

## Introduction

In grib, grid values are usually stored as

```
  grid_value = (ref_value + I*2**bin_scaling)) * 10**decimal_scaling.
    ref_value = 32-bit IEEE floating point number
    I is a positive integer
    binary_scaling = -127..127
    decimal_scaling = -127..127
```

This is not an absolute as the grid point values can be stored as
a spectral coefficients, IEEE floating point values and other formats.
The -scale option prints the binary and
scaling factors.

## Usage

```
-scale
```

### Example

```
bash-4.1$ wgrib2 gep19.t00z.pgrb2af180 -scale -for 13:14
13:473374:scale=-3,4
14:528630:scale=-1,0
bash-4.1$ wgrib2 gep19.t00z.pgrb2af180 -scale -for 13:14 -v -packing
13:473374:scale=-3,4:packing=grid point data - jpeg2000 compression,j val=(2.32109e+06+i*2^4)*10^-3, i=0..65535 (#bits=16)
14:528630:scale=-1,0:packing=grid point data - jpeg2000 compression,j val=(2215+i*2^0)*10^-1, i=0..1023 (#bits=10)
```

See also:
[-scaling](./scaling.md)
[-set_scaling](./set_scaling.md)

---

> Description: inv scale for packing

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/scale.html>_
