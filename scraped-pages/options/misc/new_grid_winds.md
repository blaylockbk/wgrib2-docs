
### wgrib2: -new\_grid\_winds



### Introduction



The -new\_grid\_winds option selects the wind rotation/orientation
for the -new\_grid option. Most users will want the winds
to be oriented to the earth's north and south directions. This is done by using the
-new\_grid\_winds earth option. Many of NCEP grids have the
winds being rotated so that north direction is relative to the grid; i.e.,
grid point (i,j) to (i,j+1). For lat-lon, Mercator and Gaussian grids, the grid
and earth relative directions are the same. For the Lambert conformal, polar
stereographic and various rotated grids, the directions are different.

To make the interpolated wind fields have a earth (grid) orientation, you have
to use the -new\_grid\_winds earth or to use the 
-new\_grid\_winds grid option before doing the interpolation.
The exception is the "-new\_grid grib". In this case, the grid definition and default
wind rotation is read from a grib file. In this case, a -new\_grid\_winds option 
will override the wind rotation read from the file.



The wind orientation applies to all the identified vector fields:
"UGRD", "VGRD", "VUCSH", "VVCSH","UFLX", "VFLX", "UGUST","VGUST","USTM","VSTM","VDFUA", "VDFVA",
 "UOGRD","VOGRD".




In the future, the wind orientation will be a part of the -new\_grid option.
However, the current systax will still work.


### Usage



```

-new_grid_winds X
    X = earth, grid

```


See also: [-new\_grid](./new_grid.html),
[-new\_grid\_vectors](./new_grid_vectors.html)
[-new\_grid\_interpolation](./new_grid_interpolation.html)










----

>Description: misc  X      new_grid wind orientation: X = grid, earth (no default)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/new_grid_winds.html>_