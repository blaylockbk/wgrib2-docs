
### wgrib2: -new\_grid intro



### Introduction


 The wgrib2 -new\_grid option can regrid grib2 files which means it can convert from one grid
to another. For example, it can change from a global 0.25 degree grid to a
one degree regional grid, or from a Lambert conformal grid to a lat-lon grid. The -new\_grid
option can change winds between earth relative and grid relative. You can use -new\_grid
to get point values using bilinear interpolation rather than the simple nearest neighbor
interpolation that the -lon uses.

### How to Interpolate


 For many packages, interpolation is simple. You give a field and the desired output
grid. Call the routine with the right arguments and the interpolation is done. No fuss and no mess.
But was there any problems?

### Problem 1: Integer values


 Some fields have integer values such as soil type and vegetation type. Fractional
values have no meaning. For such fields, you have to change the interpolation method to
nearest neighbor or most common value.

### Problem 2: Vector Fields


 Near the poles, the zonal and meridional winds are discontinous. Methods such as
bilinear interpolation have problems. One method is to convert the winds to vector,
interpolate the vectors, and then convert the interpolated vectors into the zonal
and meridional components.

### Problem 3: Global Conservation


 When changing from one global grid to another global grid, you
don't want to change the global precipitation or other global averages.
The global precipitation is one of the more difficult fields to interpolate
because of the small scale features of the precipitation.

### Problem 4: Extreme Values


 Some fields have physical limatations on the extreme values. For example,
the relative humidity will never be less than 0%, and will not be greater than 100%
except in cases of super-saturation which is unexpected for large-scale fields.
Some interpolation schemes can produce non-physical extreme values in regions of
large gradients.

### Problem 5: Ringing



Spectral interpolation methods can producing "ringing" because of 
[>Gibbs phenomenon](https://en.wikipedia.org/wiki/Gibbs_phenomenon).

### Problem 6: Unequal Variance


 With bilinear interpolation, the new grid point values
are weighted average of the 4 surrounding grid points. Suppose
we take a simple model:


```

   grid_value(i) = LS + e(i)   e(i) = Gaussian random error, mean 0, variance 1
      grib_value are the 4 surrouding grids point from the original grid

   new_grid_value = LS + sum(i=1..4) (a(i)*e(i))   where a(i) > 0 and sum of a(i) = 1
                   = LS + a * e             e = Gaussian random error, mean 0, variance 1, '
                                            0.5 <= a <= 1.0

```


From this simple model, we see that the variance from the LS will vary depending whether the new grid point
is in the exact middle of 4 points (a(i) = 0.25) or coincides with one of the points . This unequal variance can
show up in some calculations. 

### Problem 7: High to Low Resolutions: What do you want?


 Suppose your forecast model has a 0.1 degree resolution and you want
to convert it into 1x1 degree grid. Each "grid cell" of the 1x1 degree
grid will have about 100 model grid points. Do you want a grid cell
average (100 model grid points), bilinear interpolation (weighted
average of 4 model grid points) or something else. The grid cell
average will give you the large-scale field appropriate to the output grid.
The bilinear interpolation will include small scales that are not
resolved by the output grid. Some users may want the grid cell average,
some users may want the exact point values. 


### How problems are addressed with IPOLATES and wgrib2


### Answer 1: Integer-valued fields



To interpolate integer-valued fields like vegetation type and soil type, you
add code fragment to wrib2 prior to interpolation;


```

  -new_grid_interpolation bilinear \
      -if ":(VGTYP|SOTYP):" -new_grid_interpolation neighbor -fi \

  This code fragment set the interpolation type to bilinear.  For vegitation 
   and soil types, the interpolation is changed to nearest neighbor.

```


### Answer 2: Scalar vs Vector



 Wgrib2 has a built-in list of vector fields. This list can be changed by the
the [new\_grid\_vectors](./new_grid_vectors.html) option. The vectors
fields will be interpolated using vector interpolation routines in the IPOLATES library.


### Answer 3: Global Conservation



 To conserve the global mean, the interpolation needs to be changed to "budget" by


```

-new_grid_interpolation budget

```


This interpolation scheme figures out the size of the grid cell and divides in to
a 5x5 grid and finds the interpolated value on the 5x5 grid using bilinear interpolation.
The average of the 25 samples becomes the budget value. As you would expect, the budget
interpolation is much slower than the default bilinear interpolation.

### Problem 4: Extreme Values



To avoid extreme values with non-physical values, you should avoid spectral and 
bicubic interpolation methods.


### Answer 5: Ringing



Avoid spectral interpolation.


### Answer 6: Unequal Variance



 Spectral interpolation uses all the input grid points to derive any
particular output grid point. Probably a spectral interpolation with some
spectral truncation will not show this unequal variance. Of course,
answers 4 and 5 suggested that you avoid spectral interpolation, while answer 8
says the opposite. 


### Answer 7: High to Low Resolutions: What do you want?



The answer to problem 7 is to know what you want and choose the
appropriate interpolation method.

### Summary: Answers 1-7



From answers 1-7, you learn that there is no universal interpolation method.
You need to know your problem and choose the best method for that problem.




### What are Earth and Grid Relative Winds



In school, you learn that north was the direction to the North Pole which
according to Google is a small Alaskan city near Fairbanks. (Google "North
Pole location") The non-numerical weather modellers would define a V
(meridional) wind as a wind that is in the direction of the North Pole.
This definition of North which is referred to as
earth relative.


When you do numerical modelling, you think in terms of winds into the grid box.
Here you can define north to be from point (i,j) to (i,j+1). It makes life
easier when doing the calculations in the numerical model. The conversion
between earth and grid relative winds requires a rotation of the wind vector
by a location dependant angle. For the lat-lon, Mercator and Gaussian grids,
the grid and earth relative winds are the same. For the polar stereographic,
Lambert Conformal, rotated latlon grids, there is a location dependant rotation
angle.


Grib specifies whether the vectors are earth or grid relative. To transform
to earth or grid relative winds, you use the
[-new\_grid\_winds](./new_grid_winds.html) option.

Strangely
the grib standard doesn't specify which fields are vectors. 


### Specifying Earth vs Grid Relative Winds




It seems obvious that the default would to be regrid should write
earth relative winds. However, I work at NCEP and the default
at NCEP is to use grid relative winds. The only solution is to make
everyone specify whether the output uses earth or grid relative winds.
[See](./new_grid_winds.html).


```

-new_grid_winds earth
-new_grid_winds grid

```


### Example



The hard part of using the -new\_grid option is that meridional component of
the vector must follow the zonal component of the vector. However, 


```

$ wgrib2 test
1:0:d=2015101200:UGRD:1 hybrid level:anl:
2:414580:d=2015101200:PRMSL:mean sea level:anl:
3:956558:d=2015101200:UGRD:2 hybrid level:anl:
4:1357519:d=2015101200:VGRD:2 hybrid level:anl:
 Note that "UGRD:1 hybrid level" is not followed by VGRD,
 so it will not be interpolated. "UGRD:2 hybrid level" is followed by VGRD 
 and will be interpolated.
$ wgrib2 test -new_grid_winds earth -new_grid latlon 0:360:1 -90:181:1 gbl
 0:360:1 start at 0E, 360 longitudes at 1 degree spacing
 -90:181:1 start at 90S, 181 latitudes at 1 degree spacing 
1:0:d=2015101200:UGRD:1 hybrid level:anl:
2:414580:d=2015101200:PRMSL:mean sea level:anl:
-new_grid: missing V, UGRD not interpolated
 VGRD:1 hybrid level was not found (previous vector)
3:956558:d=2015101200:UGRD:2 hybrid level:anl:
4:1357519:d=2015101200:VGRD:2 hybrid level:anl:
$ wgrib2 gbl
1:0:d=2015101200:PRMSL:mean sea level:anl:
2:41287:d=2015101200:UGRD:2 hybrid level:anl:
3:73163:d=2015101200:VGRD:2 hybrid level:
 As expected UGRD 1 hybrid level is not regridded
$ wgrib2 gbl -d 1 -grid
1:0:grid_template=0:winds(N/S):
	lat-lon grid:(360 x 181) units 1e-06 input WE:SN output WE:SN res 48
	lat -90.000000 to 90.000000 by 1.000000
 Worked, earth relative winds = winds(N/S) 

```


 For of the operational NCEP model grib output, the vector fields are in
the proper order. If the vector fields are not in the correct order,
the vector fields that are not in the correct order will be ignfored
unless they are put into the right order. (See [wgrib2 tricks](https://www.ftp.cpc.ncep.noaa.gov/wd51we/wgrib2/tricks.wgrib2). If you are lucky, the vector fields that are
not in the right order is something that you don't need such as
gravity wave drag or wind stresses. 

### Simple Command to Regrid



```


 wgrib2 IN.grb -set\_grib\_type PACKING -new\_grid\_winds earth -new\_grid latlon LON0:NLON:DLON LAT0:NLAT:DLAT

    [set grib type](./set_grib_type.html) PACKING
        PACKING = s simple, fast, poor compression, very large software support
                = c1  complex 1, large software support
                = c2  complex 2, large software support
                = c3  complex 3, large software support
                = j   jpeg2000, slow, very good compression, very large software support
                = a   aec, fast, new, small software support
     [-new\_grid\_winds](./new_grid_winds.html) earth
               set output vectors to earth relative
     [-new\_grid](./new_grid.html) 
         latlon = write a lat-lon grid
         LON0 = left longitude, 0..360
         NLON = number of longitude in grid
         DLON = spacing of longitudes (degrees), greater than zero
         LAT0 = northern most or southern most latitude of grid
         NLAT = number of latitudes
         DLAT = spacing of latiudes,
                  if LAT0 is the northern most latitude, DLAT should be negative
                  if LAT0 is the northern most latitude, DLAT should be positive

```



This ends this introduction. For more information, see
the regular [new\_grid documentation](./new_grid.html),


See also: 
[-new\_grid](./new_grid.html),
[-new\_grid\_interpolation](./new_grid_interpolation.html),
[-new\_grid\_winds](./new_grid_winds.html),
[-new\_grid\_vectors](./new_grid_vectors.html),
[-lola](./lola.html),
[-bin](./bin.html),
[-import\_bin](./import_bin.html),
[-rpn](./rpn.html),
[-grib\_out](./grib_out.html),
[new\_grid multi-core usage](./new_grid_usage.html),






























```

```






















----

>Description: new_grid intro

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/new_grid_intro.html>_