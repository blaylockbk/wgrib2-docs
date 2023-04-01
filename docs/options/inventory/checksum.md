# -checksum

## Introduction

The -checksum option writes the checksum (32 bit CRC)
for the entire grib message, the decoded grid-point data or any
specified section. Two sections or messages with the same checksum are
very probably the same. If the grid-point data has the same
checksum, they are very probably bitwise identical. This option
can be used to check the integrity of a grib message or to check for
for identical sections. Note that decoded grid-point values may
not be unique. Wgrib2 is compiled with the "fast" option which may
sacrifice some precision for speed or uniqueness. For example,
A\*B\*C should be calculated by (A\*B)\*C. However A\*(B\*C) will be
faster if B\*C was previously calculated. While
mathematically the expressions are the same, the final results may
be slightly different.

## Usage

```
-checksum N
1..8 for the checksum of section N
-1 for the checksum of the entire message
data for the checksum of the decoded gridded data
```

### Example

```
$ wgrib2 test.grb2 -checksum 3
1:0:sec3_cksum=4006285726
2:4786:sec3_cksum=4006285726
3:9572:sec3_cksum=4006285726
4:13335:sec3_cksum=4006285726
5:17098:sec3_cksum=4006285726

Section 3 is the Grid Definition Section (GDS).
All 5 grib messages have the same GDS

$ wgrib2 png.grb2 -checksum -1
1:4:msg_cksum=827378178

```

```
$ wgrib2 test.grb2 -checksum 3 | cut -f3 -d: | sort -u | wc -l
 1
```

The above example prints out the number of grid defintion sections in the
file by (1) creating the checksum for the GDS, (2) extracting the GDS checksum,
(3) finding the unique checksums and finally counting them.

Space can be saved by putting combining like grib messages. For example,
if a 100 messages share the same bitmap and discipline, then the
100 messages could be combined into one message with a hundred
submessages. By combining the message, only one copy of the
bitmap is needed. This saves 99 copies of the bitmap.

```
$ wgrib2 test.grb2 -checksum 6 | sort -k3,3 -t: | wgrib2 test.grib -i -tosubmsg new.grb2
1:0:d=2008120200:TMP:800 mb:anl:
2:4786:d=2008120200:TMP:750 mb:anl:
3:9572:d=2008120200:RH:800 mb:anl:
4:13335:d=2008120200:RH:750 mb:anl:
5:17098:d=2008120200:TMP:2743 m above mean sea level:anl:

Submessage statistics:
- # submessages written  : 5
- Kbytes saved           : 0
- Kbytes written         : 20
```

See also:

```

```

---

> Description: inv X CRC checksum of section X (0..8), whole message (X = -1/message) or (X=data)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/checksum.html>_
