# -undefine_val

## Introduction

The -undefine_val option sets the grid points to undefined
depending on the value of the grid point. If a single value is specified, grid
values within 0.1 percent are set to undefined. If two values are specified,
the values that are within that range are set to undefined.

Note: the ability to handle ranges was always available but undocumented.
My mistake. That's what you get when you delay writing the documentation.

## Usage

```
-undefine_val value
   grid values within 0.1 percent are set to undefined

-undefine_val "value1:value2"
   grid value that are within the rage are set to undefined
   i.e.,   value1 <= grid_value <= value2
```

### Example

Suppose a non-standard grib file uses -999.9 as special value to
indicate an undefined grid point value. This is unfortunate as grib
readers will not know that -999.9 is a special value for undefined
values. (Grib2 uses a bitmap, NaN or out-of-range values.)
The best way to deal with these files is to convert
values of -999.9 to undefined values and then write them out.

```
$ wgrib2 IN.grb2 -undefine\_val -999.9 -set\_grib\_type same -grib\_out OUT.grb
```

See also:
[-undefine](./undefine.md),
[-ijundefine](./ijundefine.md),
[-rpn](./rpn.md),

---

> Description: misc X grid point set to undefined if X=val or X=low:high

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/undefine_val.html>_
