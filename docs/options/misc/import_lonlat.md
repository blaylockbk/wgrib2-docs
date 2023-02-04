# wgrib2: -import_lonlat

### Reading longitudes and latitudes to a file

By default grib2 saves the latitude and longitude in micro-degrees. This
is more precision than the standard single-precision floating point variable
can hold. So wgrib2 uses double precision variables for its angles.
However, the -rpn facility is single precision, so it cannot be use for
angles without losing precision.
The -export_lonlat and
-import_lonlat options allow you to write
and read double-precision longitude and latitudes.

### File format

The -import_lonlat option read the
longitudes and latitudes in the following format.

```

8 bytes:                        'wgrib2ll'       text
(sizeof unsigned int) bytes      ndata           unsigned integer with number of grid points
(sizeof unsigned int) bytes      0               unsigned integer with value of zero
ndata*(sizeof double)            longitudes      ndata values of double precision longitudes
ndata*(sizeof double)            latitudes       ndata values of double precision latitudes

(sizeof unsigned int) is usually 4.  By wgrib2 requirements, the value must be 4 or greater.
(sizeof double) is usually 8.

```

Prior to wgrib2 v3.1.2, -import_lonlat would always
read the first record of the input file. For wgrib2 v3.1.2+,
-import_lonlat would read the records sequentially. If
there was an EOF, then read would start at the beginning of the file. Consequently
old behavior mimicked for files with only one record.

### Caution

The -import_lonlat will change the internal values of
the longitudes and latitudes. However, wgrib2 will still use the original
grid organization. For example, the grib message was a lat-long grid,
and replaced the values for a polar stereographic grid. Some parts of wgrib2
may still assume the grid is a lat-lon grid.
The -import_lonlat will be more useful for unstructured grids
like GDT 3.101 (General Unstructured Grid). GDT 3.101 does not include geolocation,
and -import_lonlat can add the geolocation. Consequently
the nearest neighbor interpolation will then work.

## Usage

```

-import_lonlat FILE
    FILE = file that has special format with longitude and latitude values.
    The longitude and latitudes replace the internal values.

```

### Example

```

   $ wgrib2 IN.grb -ncpu 3 -new_grid_winds grid -new_grid ncep grid 221 - | wgrib2 - -ncpu 1 -set_grib_type j -ncep_uv OUT.grb

```

The above line uses 3 threads for regridding and one thread for jpeg2000 compression. The jpeg2000
compression routines can't take advantage of more than one thread.

See also:

---

> Description: misc X read lon-lat data from binary file

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/import_lonlat.html>_
