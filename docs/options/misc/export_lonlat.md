# wgrib2: -export_lonlat

### Writing longitudes and latitudes to a file

By default grib2 saves the latitude and longitude in micro-degrees. This
is more precision than the standard single-precision floating point variable
can hold. So wgrib2 uses double precision variables for its angles.
However, the -rpn facility is single precision, so it cannot be use for
angles without losing precision.
The -export_lonlat and
-import_lonlat options allow you to write
and read double-precision longitude and latitudes.

### File format

The -export_lonlat option writes the
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

## Usage

```

-export_lonlat FILE
    FILE = file that is written with the binary data

```

### Example

```

   $ wgrib2 IN.grb -ncpu 3 -new_grid_winds grid -new_grid ncep grid 221 - | wgrib2 - -ncpu 1 -set_grib_type j -ncep_uv OUT.grb

```

The above line uses 3 threads for regridding and one thread for jpeg2000 compression. The jpeg2000
compression routines can't take advantage of more than one thread.

See also:

---

> Description: misc X save lon-lat data in binary file

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/export_lonlat.html>_
