# wgrib2: -g2clib

## Introduction

Originally wgrib2 used the g2clib library for unpacking the grid point data (GDS).
The first break from a complete dependence on the g2clib was adding support
for ieee-type grib files. Then the optimized grib1-style unpacking from wgrib
was added to the internal routines. Then jpeg, png and complex packing were added
to the internal decoder. Finally more packing schemes that were not in g2lib/g2clib
were added (run-length and log scaling).

As of wgrib2 v1.9.8, a third option was added, use the internal decoder in g2clib
emulation mode. The options were also relabeled. The new default is to use
the internal decoder in g2clib emulation mode (-g2clib 1).
To use use the internal decoder you still use -g2clib 0.
To get the g2clib decoder, use -g2clib 2 which is now
a compile-time option. The changes were made because

1. internal routines are faster for complex packing (1 cpu)

- some internal routines were parallelized for decoding (complex and simple packing)
- some distributions were linking the official g2clib rather than the patched
  version included with the wgrib2 sources. This caused seg faults.

Wgrib2 has always used its own routines for encoding grib files. The
encoding follows the WMO standard but are compatible with the NCEP libraries
(g2clib and g2lib).

### Differences as of October 2008

The NCEP encoding/decoding routines have a problem
with constant fields. The routines ignore the decimal scaling
and assume it is zero (decimal scaling = multiplication by one).
The packing and unpacking routines are are consistent
with themselves so the problem wasn't noticed for many years.
As of 6/2014 this situation hasn't changed. This same bug
has been found in routine in other software packages. Some
are consistently ignore the decimal scaling for all packing modes,
whereas others only have the bug for certain packing modes.
By the use of "-g2clib 0" and "-g2clib 1", you can handle
grib files without/with this bug.

The wgrib2 internal decode routines do not have a decimal scaling
problem and follow the WMO standard. The wgrib2 encode routines
will automatically use a decimal scaling = 0 for constant fields
and consequently be compatible with the WMO standard and will
decode correcty with g2clib/g2lib.

The g2clib emulation (default, -g2clib 1) are recommend when

1. The files were created by using g2clib/g2lib libraries (NCEP)

- The files are created using software with the constant-field/non-zero decimal scaling bug

Turning off the g2clib emulation (-g2clib 0) is recommend when

1. grib files follow WMO standard for constant fields

Finally one can use the g2clib routines (an optional compile time option) by the
setting -g2clib 2 on the wgrib2 command line. This is useful for testing and
for cases where g2clib supports a packing that is not supported by the
internal decoder. The disadvantages of the g2clib routines are

1. Single precision routines

- The routines are slower for large grids and they are not threaded
- The g2clib routines "fail" when they finds an undefined template.
- Decoding problems with certain fields (6/2016).
- Problems with seg faults when linked with the official g2clib (Fedora/Redhat rpms)

### -g2clib

The -g2clib option allows you to select the internal/g2clib
decoder. If X is 0, then internal routines are selected.
If X is 1, then the internal decoder with g2clib emulation is selected (g2clib is used with older wgrib2 versions).
When X is 2, then the g2clib decoder is used if the g2clib decoder were compiled into the executable.
Note that g2clib doesn't support ieee, RLE, AEC and irregular structured grids.

## Usage

```

-g2clib N   N = 0, 1 or 2
            0 = WMO standard
            1 = emulate NCEP g2/g2c bug for decoding constant values fields (default)
            2 = use g2clib, only available if compiled with g2clib
                This option should only be used for testing.
                Some distributions will seg fault using this option.

```

---

> Description: init X X=0/1/2 0=WMO std 1=emulate g2clib 2=use g2clib

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/g2clib.html>_
