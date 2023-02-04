### pywgrib2: global variables

## Introduction

Pywgrib_s has two types of global variables. The first type is the
output of the pywgrib2_s.inq(..) routine, such as the size of
the grid, the grid values and grid locations. The second type
are the configuration options of pywgrib2_s.

### Output global variables

1. nx: integer, size of matched grid in zonal direction

- ny: integer, size of grid in meridional direction
- ndata: integer, size of grid
- nmatch: integer, number of matches by inq(..)
- msgno: integer, grib message number
- submsgno: integer, submessage message number
- data: float32s, grid point values
- lat: float32s, grid point latitudes
- lon: float32s, grid point longitudes
- secN: bytes, byte values of secN N=0..8
- matched: list of strings of matched fields
- grid_defn: string of grid definition

### Configuration global variables

1. use_numpy_nam: logical, undefined grid value is numpy.nan (true) or 9.999e20 (False)

- names: string, either 'ncep', 'ecmwf' or 'dwd', convention for WMO grib names
- debug: logical, print debug statements (True)

[overview](./pywgrib2_s.md)

---

> Description:

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pywgrib2_s_global_variables.html>_
