# wgrib2: -set_metadata_str

## Introduction

The -set_metadata_str "string" option is similar to the
older -set_metadata FILE option. Instead of reading
the metadata from a file, the metadata is on the command line. The latter
option is generally more useful as each grib message can have its own
set of metadata. The former option is only useful for specifying the metadata
for a single grib message. The -set_metadata_str option
was added to facilitate the creation of an "callable wgrib2" API for writing grib2.
See [-set_metadata](./set_metadata.md) for the format of the metadata string.

## Usage:

```
-set_metadata_str "metadata"
metadata is a string
```

### Example:

```
sh-4.1$ wgrib2 small.grb2 -set\_metadata\_str "1:0:d=2001020304:TMP:10 mb:anl:"
1:0:d=2001020304:TMP:10 mb:anl:ENS=+19

```

See also:
[-set_metadata](./set_metadata.md),

---

> Description: misc X X = metadata string

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_metadata_str.html>_
