
### wgrib2: -cyclic



### Introduction



Sometimes it is useful to know whether the domain is cyclic.
For example, the large domain goes from 0E to 359W by steps
of 1 degree. Now there is a request for a subdomain that goes
from 350E (left) to 10E (right). Knowing that the domain is cyclic
is useful in this case. The -small\_grib option 
calls the cyclic code.

 The -cyclic option will only detect cyclic
grids that are either lat-lon, Gaussian or Mercator. All other
grids will return a not-cyclic response. In addition,
staggered and thinned grids will also return a not-cyclic response.
The test for cyclic is a bit loose because NCEP grib2 files often
have the lon(gitude) and dlon(gitude) to the nearest millidegree rather than the
nearest microdegree. (Reason 1: grib1 only stored the lon and dlon to the
nearest millidegree. Reason 2: most NCEP codes use single precision
for the longitudes and latitudes.)

### Usage



```

-cyclic

```

### Example




```

$ wgrib2 gdt140.g2 -cyclic -nl -grid
1:0:not cyclic:
:grid_template=140:winds(N/S):
	Lambert Azimuthal Equal Area grid: (1050 x 1050) input WE:NS output WE:SN res 48
	Lat1 18.876988 Lon1 225.000000 Cen Lon 0.000000 Std Par 90.000000
	Dx 10000.000000 m Dy 10000.000000 m mode 48

$ wgrib2 gep19.t00z.pgrb2af180 -cyclic -nl -grid
1:0:cyclic:
:grid_template=0:winds(N/S):
	lat-lon grid:(360 x 181) units 1e-06 input WE:NS output WE:SN res 48
	lat 90.000000 to -90.000000 by 1.000000
	lon 0.000000 to 359.000000 by 1.000000 #points=65160
...

```


See also: 
[-small\_grib](./small_grib.html),










----

>Description: inv          is grid cyclic? (not for thinned grids)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/cyclic.html>_