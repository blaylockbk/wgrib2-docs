# -bitmap

## Introduction

GRIB2 has 3 ways to specify undefined values. If the grid point
data is stored as IEEE format numbers, then the IEEE format has NaN (not
a number). NaN values can considered to be a undefined value. For
complex packing, values that are outside the range of normal
values are undefined. Decoders can give these undefined values a
special value. All the packing methods support a bitmap for specifying
undefined values.

The -bitmap option prints whether the record has a bitmap
and the number of undefined points as specified by the bitmap. It is possible
for an IEEE format packing to defined undefined by both NaN and a bitmap. So
in this case the number of undefined values will be larger than specified by
the bitmap. The same is true for complex packing where a bitmap could be
combined with special values undefineds. Combining two methods of specifying
undefineds would increase the file size, so it is not recommended.

Note that using a bitmap to specify undefined values is not as efficient
as using special-value undefineds. The file size is significantly bigger and
decoding can be slower (wgrib2's code is parallelized for special values).

Note that wgrib2 uses 9.999e20 for all undefined values including
special value undefineds. (The special value is ignored.)

## Usage

```
-bitmap
```

### Example

```
$ wgrib2 grib2.polar -bitmap
1.1:0:no bitmap
1.2:0:no bitmap
```

See also:

---

> Description: inv bitmap mode

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/bitmap.html>_
