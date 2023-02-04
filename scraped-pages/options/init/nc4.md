# wgrib2: -nc4, -nc3

## Introduction

Wgrib2 can be compiled to use either the netCDF v3 (netCDF3) or netCDF v4 (netCDF4) libraries. If wgrib2 is
compiled with the netCDF3, you can neither read nor write netCDF4 files. However, if wgrib2 is
compiled with netCDF4, wgrib2 can read and write netCDF3 and netCDF4 files. The disadvantage of
linking in netCDF4 is that the wgrib2 executable is much larger, compiling wgrib2 can now take hours
(1.1 GHz Apollo Lake),
and problems have been encountered in compiling the latest hdf5 library with newer compilers (12/2018).

The -nc4 and
-nc3 options toggle the type of netcdf file written. The former
option will have the netcdf file be netcdf-4 format with compression turned on. The latter
will have the netcdf file be uncompressed in netcdf-3 format.

### Simple usage

```

-nc3
-nc4

```

### Example 1

```

$ wgrib2 ../example/eta.t00z.awphys18.grb2 -nc4 -netcdf eta.nc

The above line converts the grib2 file into a netcdf4 file assuming wgrib2 was
compiled with netCDF4.


```

See also:
[-netcdf](./netcdf.html),
[-nc3](./nc3.html)
[-nc4](./nc4.html)

---

> Description: init use netcdf4 (compressed, controlled endianness etc)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/nc4.html>_
