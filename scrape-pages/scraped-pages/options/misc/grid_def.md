# wgrib2: -grid_def

## Introduction

When wgrib2 processes a field, it often calculates the longitude
and latitude (location) of each grid point. Wgrib2 doesn't do the calculation
when one of the following conditions are true

1. the locations of the grid points are not needed by any of the options

- the grid is the same as the previously processed grib message
- wgrib2 does not know how to calculate the locations

For some grids, wgrib2 does not know how to calculate the grid locations
and the grid locations are available from the center in the form of grib files.
Then you can use the -grid_def to add the grid
locations for wgrib2 processing.
The option, -grid_def checks to see if the grib message
is a longitude or latitude. If it is, the longitude or latitude is associated
with the longitude or latitude of the grid points.
Because of "2", wgrib2 will use these longitudes
or latitudes for the following fields until wgrib2 encountours a different
grid.

### wgrib2 up to v2.0.5

Bug in the -grid_def option only allows
the option to work with grids with calculated grid locations.
(Passed testing but not that useful.)

### wgrib2 v2.0.6, v2.0.7

1. LOUV, LOPP or ELON will associate the data with the longitudes

- LAUV, LAPP or NLAT will associate the data with the latitudes

### wgrib2 v2.0.8+

1. LOUV, LOPP, ELON or GEOLON will associate the data with the longitudes

- LAUV, LAPP, NLAT or GEOLAT will associate the data with the latitudes

### wgrib2 v3.0.0+

With wgrib2 v3.0.0 can associated latitudes and longitudes using -rpn
and the "sto_lat" and "sto_lon" options. You can import double precision
lat and lons using -import_lonlat.

### Precision of the latitudes and longitudes

By default, grib2 stores angles to the millionth of a degree. This
requires the angles to be stored in double precision. Reading the
latitudes and longitudes in grib format could be done in double
precision but the current decoder is limited to 25 bits which is
basically single precision. If you need double precision lat and lon
values, use -import_lonlat.

## Usage

```
-grib_def
  will alter the latitude or longitudes associated with the grid points
  when the variable is a latitude or longitude field
```

### Example

For -grid_def to work, the latitude and longitudes have
processed prior the to remaining grids. The file, icon.grb, satifies this requirement.

```
$ wgrib2 icon.grb
1:0:d=2019040900:GEOLON:surface:anl:
2:5898409:d=2019040900:GEOLAT:surface:anl:
3:11796818:d=2019040900:TMP:2 m above ground:0-360 min max fcst:
```

An additional requirement is the grids be the same.

```
$ wgrib2 icon.grb -grid
1:0:grid_template=101:
	General Unstructured Grid grid=26 ref_grid=1 uuid=a27b8de6-18c4-11e4-820a-b5b098c6a5c0

2:5898409:grid_template=101:
	General Unstructured Grid grid=26 ref_grid=1 uuid=a27b8de6-18c4-11e4-820a-b5b098c6a5c0

3:11796818:grid_template=101:
	General Unstructured Grid grid=26 ref_grid=1 uuid=a27b8de6-18c4-11e4-820a-b5b098c6a5c0
```

A more precise method of identifying identical grids is by

```
$ wgrib2 icon.grb -checksum 3     (Section 3 is the grid defininition template)
1:0:sec3_cksum=470959557
2:5898409:sec3_cksum=470959557
3:11796818:sec3_cksum=470959557
```

Using wgrib2 v3.0.0+, we can get a 2x2 degree lat-lon grid by,

```
$ wgrib2 icon.grb -grid\_def -not\_if ":GEO(LAT|LON):" -s -lola 0:180:2 -90:91:2 lola.grb grib -endif
1:0
2:5898409
3:11796818:d=2019040900:TMP:2 m above ground:0-6 hour max fcst:
bash-4.1$ wgrib2 lola.grb -s -grid
1:0:d=2019040900:TMP:2 m above ground:0-360 min max fcst::grid_template=0:winds(N/S):
	lat-lon grid:(180 x 91) units 1e-06 input WE:SN output WE:SN res 48
	lat -90.000000 to 90.000000 by 2.000000
	lon 0.000000 to 358.000000 by 2.000000 #points=16380

The -not_if is used so that GEOLAT and GEOLON fields are not interpolated.
```

For wgrib2 up to v2.0.6 and, -grid_def did not recognize GEOLAT and GEOLON.
As a result, the location had to be converted to NLAT and ELON which are
recognized. NLAT and ELON are NCEP local variables, so the center has to
be changed to 7 which is NCEP by the GRIB standard.

```
$ wgrib2 icon.grb \
 -if ":GEOLAT:" -set center 7 -set\_var NLAT -fi \
 -if ":GEOLON:" -set center 7 -set\_var ELON -fi \
 -grid\_def -s \
 -not\_if ":GEO(LAT|LON):" -s -lola '0:180:2' '-90:91:2' lola.grb grib
1:0:d=2019040900:ELON:local level type 1 0:anl:
2:5898409:d=2019040900:NLAT:local level type 1 0:anl:
3:11796818:d=2019040900:TMP:local level type 103 2:0-6 hour max fcst::d=2019040900:TMP:local level type 103 2:0-6 hour max fcst:
```

With wgrib2 v3.0.0, you can also use -rpn to update the latitude and
longitudes.

```
$ wgrib2 icon.grb icon.grb -if ":GEOLAT:" -rpn sto\_lat -endif -if ":GEOLON:" -rpn sto\_lon -endif \
 -not\_if ":GEO(LAT|LON):" -s -lola "0:180:2" "-90:91:2" lola.grb grib -endif
```

See also:
[-rpn](./rpn.md)

```

```

---

> Description: misc read lon and lat data from grib file -- experimental

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/grid_def.html>_
