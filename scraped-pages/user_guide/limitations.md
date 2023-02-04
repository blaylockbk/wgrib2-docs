# wgrib2: limitations

Limitations on 32-bit computer 2. The maximum grib file is limited to 2GB unless the file is read sequentially.

i.e., cat grib_file | wgrib2 - (rest of options)

- large grib messages and grids will use buffer space and exhaust physical memory.
- grib message is limited to 2GB (fixed v2.0.4)
- Any buffer greater than 4GB is a problem.
- The maximum grib message is limited to 2GB to 4GB (depending on routine)
- The maximum number of grid points varies by routine from to 2G to 4G (grib2 standard=4G).

The amount of addressable memory will be a limitation.

- The maximum bit precision of a complex-packed file is 25.
  Limitations on 64-bit computer

2. The maximum number of grid points varies by routine from to 2G to 4G (grib2 standard=4G).

Low priority to get the routines to 4G.

- -new_grid is limited to 2G grid points by the fortran library.
- grib message is limited to 2GB (fixed v2.0.4)
- The maximum bit precision of a complex-packed file is 25.
  Limitations on computer with 64-bit pointers and 32-bit long integers

2. Similar to 32-bit limitations.
   Supported Unpacking Schemes
3. simple

- simple with log preprocessing
- complex with bitmaps or special values
- jpeg2000
- png
- ieee
- RLE
- CCSDS/AEC
  Supported Packing Schemes

2. simple

- complex with bitmaps or special values
- jpeg2000
- ieee
- CCSDS/AEC
  Grid Support Levels: geolocation

2. nothing

- diagnostic inforamtion
- latitude/longitude of the grid points are calculated (spherical earth)
- latitude/longitude of the grid points are calculated (aspherical earth)
  Grid Support Levels: ipolates (-new_grid)

2. nothing

- can be an input grid for -new_grid
- can be an input or output grid for -new_grid
  Grid Support Levels: -small_grib

2. nothing

- can be an input grid for -small_grib
  Grid support:

* lat-lon: full support
* Lambert conformal: full support
* Polar stereographic: full support
* Mercator: full support
* Gaussian: all but -small_grib
* thinned lat-lon grids: only geolocation
* rotated lat-lon grids: only geolocation
* space view perspective: only geolocation
* Albers equal area: only geolocation
* Lambert Azimuthal equal area: only geolocation
* staggering all above except for Gaussian: only geolocation
* irregular grid: only geolocation

|

|     |
| --- |

|

---

|
| [NOAA/](https://www.noaa.gov/)
[National Weather Service](https://www.nws.noaa.gov/)
[National Centers for Environmental Prediction](https://www.ncep.noaa.gov/)
Climate Prediction Center
5830 University Research Court
College Park, Maryland 20740
[Climate Prediction Center Web Team](/comment-form.html)
Page last modified: July 27, 2016
| [Disclaimer](https://weather.gov/disclaimer.php) | [Privacy Policy](https://weather.gov/privacy.php) |

|

---

> Description: limitations

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/limitations.html>_
