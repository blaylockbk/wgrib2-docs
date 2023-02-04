# wgrib2: -geolocation

## Introduction

Geolocation routines are used to calculate the longitudes and latitudes
of the grid points. Wgrib2 can use Proj4, gctpc and its own internal
routines to do the geolocation. Each set of routines has advanages and
disadvantages. The routines are listed in the default order.

### Proj

The Proj library was called PROJ4 from 1994-2017. In 2018, PROJ v5 was released
and the software was renamed PROJ. Wgrib2 is still using a v4 library and
all references in the code are to PROJ4.

PROJ is the gold standard of geolocation libraries. It is commonly used
and well supported. The drawbacks are it doesn't support OpenMP and
model grids. As of wgrib2 v3.1.2, PROJ4 is installed by default in order
to suppore aspherical equal area Lambert grids.

### Gctpc

Gctpc is an older library was which has lost support of its original authors (USGS).
The advantage is that it does support OpenMP unlike Proj4. It supports fewer grids
than Proj4 but that isn't a major problem because the grib2 standard only has
support for a few grids. Gctpc supports an ellipsoidal earth and
is always compiled with wgrib2.

### Internal

The internal routines are used to support model grids (Gaussian grid, rotated lat-lon grid),
spherical earth grids, and observational grids such as the space-view grid (used by EUMETSAT)
and radar grids (angle, distance from the origin).

### External, wgrib2 v3.1.2+

Wgrib2 can set or overwrite the locations of the grid points using -rpn
(sto_lat, sto_lon), reading from a grib file using -grid_def, and reading from
a double precision binary file using -import_lonlat. These methods will
set the geolocation flag to external.

For wgrib2 prior to v3.1.2, the geolocation flag showed which geolocation library
generated the locations of the grid points. External methods were not considered.

## Usage

```

-geolocation
    print out geolocation routines used: proj4, gctpc, internal, external, or not_used
      external means the lat/lon may be read or computed by an external calculation.
      If wgrib3 does not need the geolocation information, the geolocation routines
         will not be called.

```

### Example

```

$ wgrib2 burned\_area\_20120131\_12\_23\_1km.grib2 -geolocation -lon -90 30
1:0:geolocation=gctpc:lon=270.004694,lat=29.997210,val=0

	The gctpc routines were used for geolocation.

$ wgrib2 burned\_area\_20120131\_12\_23\_1km.grib2 -geolocation
1:0:geolocation=not_used

	The geolocation use not used.

```

See also:
[-gctpc](./gctpc.html)
[-grid_def](./grid_def.html)
[-import_lonlat](./import_lonlat.html)
[-proj4](./proj4.html)
[-rpn](./rpn.html)

---

> Description: inv package (proj4,gctpc,internal,not_used) to get lat/lon of grid points

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/geolocation.html>_
