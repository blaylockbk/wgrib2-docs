# -ftime2

## Introduction

The -ftime option calls either
-ftime1 or -ftime2 options.
The -ftime2 option is newer and will become the
default in newer versions of wgrib2. The -ftime2 option
fixes some problems in describing climatologies of monthly means (averages of averages)
or any nested statistical processing operators. The
-ftime2 option is fully recursize while the older
-ftime1 attempted and failed to bypass the recursion.
Along with the change to -ftime2, the
The -set_ftime, -set_ave options are being replaced
by -set_ftime2.

## Usage

```
-ftime2
```

### Example

```
$ wgrib2 grib2.polar -ftime2
1.1:0:24 hour fcst
1.2:0:24 hour fcst
```

See also:
[-set_ftime](./set_ftime.md)
[-vt](./vt.md)

---

> Description: inv timestamp -- will replace -ftime in the future TESTING

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ftime2.html>_
