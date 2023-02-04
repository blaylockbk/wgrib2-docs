# wgrib2: -set_grib_type

## Introduction

wgrib2 can write grib files by either copying an existing
packed grib message (-grib) or by
taking floating point values and packing them into a new grib
message. (For example, -grib_out,
-small_grib, -ij_small_grib,
and -wind_speed).
For the latter, one has a choice of packing methods.
Generally methods that are faster methods create larger files
and slower methods make smaller files. For fields with no
undefined values, the jpeg2000 produces the smallest files.
With bitmaps, one of the complex packings is usually the best
(of the supported packing). Complex3 is often the best trade off
between speed and compression for smooth fields. For other fields
complex1 is often the best trade off between speed and compression.
The (-set_grib_type) option controls the
packing method.

Complex packing has the option of using special values
or bitmap for undefined grid values. Special values produce
significantly smaller files than using the bitmap. You should
only use the bitmap and complex packing when trying to remain
compatible with certain codes. (Bitmaps and special values were
part of the original grib2 specification, so it is possible that
some codes can handle special values and not bitmaps.) If you want
to alter the default (special values) to bitmaps, you
can use the option -set_bitmap.

AEC is an implementation of CCSDS compression using libaec.
This new compression has very fast compression speeds and good
compression. As of 7/2016, only ecCodes (ECMWF) and wgrib2 support
AEC compression. More support is expected after it is
part of the offical standard (expected 9/2016).

## Usage

```

-set_grib_type X   X=ieee,simple,complex1,complex2,complex3,jpeg,aec,same
                     s, c1, c2, c3, j, a are short for simple .. aec
                     v2.0.7 adds complex1-bitmap, complex2-bitmap, complex3-bitmap
                       with c1b, c2b and c3b being the short forms



ieee = data is ieee format (4 bytes per data point)
simple = no compression, packed scaled integers
complex1 = complex packing
complex1-bitmap = complex and using bitmap for undefined grid points
complex2 = complex packing, pack increments (deltas)
complex2-bitmap = complex packing, pack increments (deltas) and using bitmap for undefined
complex3 = complex packing, pack increments after linear extrapolation
complex3-bitmap = complex packing, pack increments after linear extrapolation and using bitmap for undefined
jpeg = jpeg2000 compression
aec = aec/CCSDS compression
same = try to keep same packing type as input
       if input is in an unsupported output packing, complex1 is used

note: to use bitmap for undefined values in complex packing, you can use
 -set_bitmap 1 or you can use the complexN-bitmap.

```

### Example

```

$ wgrib2 in.grb -set\_grib\_type complex3 -grib\_out out.grb

```

The above line rewrites a file using complex3 packing.

See also: [-grib_out](./grib_out.md),
[-set_bitmap](./set_bitmap.md),

---

> Description: misc X set grib type = jpeg, simple, ieee, complex(1|2|3), aec, same

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_grib_type.html>_
