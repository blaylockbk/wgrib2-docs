# wgrib2: -grid

## Introduction

The -grid option prints out the grid information.

```

$ wgrib2 gep19.t00z.pgb2af180 -grid -d 1
1:0:grid_template=0:winds(N/S):
	lat-lon grid:(360 x 181) units 1e-06 input WE:NS output WE:SN res 48
	lat 90.000000 to -90.000000 by 1.000000
	lon 0.000000 to 359.000000 by 1.000000 #points=65160

```

```

$ wgrib2 nam.683 -grid
1:0:grid_template=30:winds(grid):
	Lambert Conformal: (1473 x 1025) input WE:SN output WE:SN res 8
	Lat1 12.190000 Lon1 226.541000 LoV 265.000000
	LatD 25.000000 Latin1 25.000000 Latin2 25.000000
	LatSP 0.000000 LonSP 0.000000

```

```

$ wgrib2 .t00z.master.grb2f048 -grid -d 1
ebis@landing2:~/grib2_examples$ wgrib2 gfs.t00z.master.grb2f048 -grid | more
1:0:grid_template=40:winds(N/S):
	Gaussian grid: (3072 x 1536) units 1e-06 input WE:NS output WE:SN
	number of latitudes between pole-equator=768 #points=4718592
	lat 89.909340 to -89.909340
	lon 0.000000 to 359.882813 by 0.117188

```

```

$ wgrib2 merc.g2 -grid -d 1
1:0:grid_template=10:winds(N/S):
	Mercator grid: (73 x 23) LatD 22.500000 input WE:SN output WE:SN res 48
	lat -48.090000 to 48.090000 by 513669.000000 m
	lon 0.000000 to 0.000000 by 513669.000000 m
	orientation 0.000000

```

The four previous examples are for a grid definitions of a lat-lon, Lambert Conformal,
Gaussian and Mercator grids. These are the most common grids that are commonly distributed
from NCEP. Other commonly used grids are: polar stereographic, rotated lat-lon and
thinned Gaussian. Radar and satellites often use different grids.

### Understanding the grid definitions

The grid definitions are based on the grib grib defintions as published
by the WMO and copied by the NCEP's grib documentation. For example, the
lat-lon grid defintion is given by
<https://www.nco.ncep.noaa.gov/pmb/docs/grib2/grib2_doc/grib2_temp3-0.shtml>.
Wgrib2's version of the lat-lon grid is given by

```

$ wgrib2 gep19.t00z.pgb2af180 -grid -d 1
1:0:grid_template=0:winds(N/S):
	lat-lon grid:(360 x 181) units 1e-06 input WE:NS output WE:SN res 48
	lat 90.000000 to -90.000000 by 1.000000
	lon 0.000000 to 359.000000 by 1.000000 #points=65160

template=N      .. grid defintion template 3.N
winds(N/S)      .. winds are earth relative
winds(grid)     .. winds are grid relative
(NX x NY)       .. grid dimensions (for rectangular grid)
units 1e-06     .. scaling of angles, scaled angles are stored as 4-byte integers
input WE:NS     .. grib has the data in WE:NS scan order
output WE:SN    .. wgrib2 internally has the data is in WE:SN scan order
                   if wgrib2 writes a bin/ieee/text file, it will be in the output scan order
                   The output scan order has to be WE:SN for geolocation to work.
                   The default output scan order is WE:SN.
output WE:NS    .. wgrib2 internally has the data is in WE:NS scan order
                   The is enabled by -order we:ns, and is used for writing binary data
                   in WE:NS order.
output raw      .. wgrib2 internally has the data in the input scan order
                   This scan order is needed for -new_grid to work.
res N           .. value of the resolution and component flags octet (byte)
lat X to Y by Z .. latitudes start at X goes to Y by steps of Z
lon X to Y by Z .. longitudes start at X goes to Y by steps of Z
                   note: grib2 uses longitude is in [0,360) model
#points N       .. number of points in the grid including grid points with undefined values

```

### Staggered Grids, wgrib2 2.0.8+

Staggered grids are often used in grid point (as opposed to spectral)
atmospheric models. (Arakawa, A.; Lamb, V.R. (1977). "Computational design of the
basic dynamical processes of the UCLA general circulation model". Methods in Computational Physics:
Advances in Research and Applications. 17: 173–265.) There are advantages in
storing the model grids in grib for both the modeler and the user. The advantages
are compactness, and a standard format. The user also has the advantage of
eliminating an extra interpolation step. The staggering information is stored
in the last 4 bits of flag table 3.4. If these bits are all zero, there is no
staggering.

Staggered grids in GRIB work by

1. A "fundamental" grid is defined

- staggered grid could have an 0 dx offset in the X direction
- or staggered grid could have an 1/2 dx offset in the X direction
- or staggered grid could have 0 dx offset for odd rows and 1/2 dx for even rows (all in the X direction)
- staggered grid could have an 0 dy offset in the Y direction
- or staggered grid could have an 1/2 dy offset in the Y direction
- staggered grid could have fewer points in the row or column if the offset is non-zero

This scheme allows encoding the Arakawa A-E Egrids. The wgrib2 -grid will show
the staggering. The storage description was updated in v2.0.8 to be

1. nx\*ny: length of the row is nx, there are ny rows

- nx\*(ny-1): length of the row is nx, there are ny-1 rows
- trim-x\*ny: length of the row is either nx or nx-1, there are ny rows
- trim-x\*(ny-1): length of the is either nx or nx-1, there are ny-1 rows

## Usage

```

-grid

```

See also:
[-nxny](./nxny.md),
[-nlons](./nlons.md)

---

> Description: inv grid definition

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/grid.html>_
