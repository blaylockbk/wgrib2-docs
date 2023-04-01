# -nlons

## Introduction

The -nlons option prints the number
of grid points for each row of the grid. This
is necessary for a thinned grid where the number of
grid points per row decreases as you approach the poles.

## Usage

```
-nlons
```

### Example

```
png.grib is on a 360x181 grid

$ wgrib2 -grid png.grb2
1:4:grid_template=0:winds(N/S):
	lat-lon grid:(360 x 181) units 1e-06 input WE:NS output WE:SN res 48
	lat 90.000000 to -90.000000 by 1.000000

$ wgrib2 -nlons png.grb2
1:4:nlon (S/N)=360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360 360

Each row has 360 grid points.

reduced_gaussian_surface_jpeg.grib2 is on a Gaussian thinned (reduced) grid.

$ wgrib2 reduced\_gaussian\_surface\_jpeg.grib2 -grid
bash-4.1$ wgrib2 reduced_gaussian_surface_jpeg.grib2 -grid
1:0:grid_template=40:winds(N/S):
	thinned global Gaussian grid: (-1 x 64) units 1e-06 input WE:NS output raw
:: NX = -1 because the number of grid points varies by latitude
	number of latitudes between pole-equator=32 #points=6114
	lat 87.864000 to -87.864000
	lon 0.000000 to 357.188000 by -2147.483647
	#grid points by latitude: 20 27 36 40 45 50 60 64 72 75 80 90 90
	 96 100 108 108 120 120 120 128 128 128 128 128 128 128 128 128 128 128 128 128
	 128 128 128 128 128 128 128 128 128 128 128 120 120 120 108 108 100 96 90 90
	 80 75 72 64 60 50 45 40 36 27 20
:: number of grid points varies from 20 to 128.

$ wgrib2 reduced\_gaussian\_surface\_jpeg.grib2 -nlons
1:0:nlon (S/N)=20 27 36 40 45 50 60 64 72 75 80 90 90 96 100 108 108 120 120 120 128 128 128 128 128 128 128 128 128 128 128 128 128 128 128 128 128 128 128 128 128 128 128 128 120 120 120 108 108 100 96 90 90 80 75 72 64 60 50 45 40 36 27 20
```

See also:
[-grid](./grid.md)

---

> Description: inv number of longitudes for each latitude

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/nlons.html>_
