# wgrib2: -radius

## Introduction

Most grib2 Grid Definition Templates (PDT) include the
shape and dimensions of Earth. Such information is
necessary for finding the latitude and longitude of
the grid points in various projections. The
-radius option shows the
shape and size of Earth.

## Usage

```

-radius

```

### Example

```

$ wgrib2 -radius png.grb2
1:4:code3.2=6 sphere predefined radius=6371229.0 m

```

See also:
[-set_radius_inv](./set_radius.html)

---

> Description: inv radius of Earth

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/radius.html>_
