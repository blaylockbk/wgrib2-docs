# wgrib2: -set_radius

## Introduction

Most grib2 Grid Definition Templates (PDT) include the
shape and size of Earth. Such information is
necessary for finding the latitude and longitude of
the grid points in various projections. The
-set_radius option sets the
shape and size of Earth and the -radius option
shows the the shape and size.

The geolocation of the grid points is done prior to the execution of
the run-time options. So the
-set_radius will not affect calculations of
the lat-lon of the grid points. To get the correction locations
after using the
-set_radius option, you must write the file
with the new shape of the Earth. Then you can use this new file.

## Usage

```

-set_radius N      N=0,2,4,5,6,8,9
                   Code Table 3.2 is set to N
-set_radius 1:R    R=radius in meters (spherical)
                   Code Table 3.2 is set to 1
-set_radius 3:X:Y  X=major axis Y=minor axis (oblate spheroid), X, Y in km
                   Code Table 3.2 is set to 3
-set_radius 7:X:Y  X=major axis Y=minor axis (oblate spheroid), X, Y in m
                   Code Table 3.2 is set to 7

```

### Example

```

$ wgrib2 small.grb2 -set\_radius 0 -radius
1:0:code3.2=0 sphere predefined radius=6367470.0 m
$ wgrib2 small.grb2 -set\_radius 1:6300000 -radius
1:0:code3.2=1 sphere user defined radius=6300000.0 m

```

See also:
[-radius](./radius.md)

---

> Description: misc X set radius of Earth X= 0,2,4,5,6,8,9 (Code Table 3.2), X=1:radius , X=7:major:minor

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_radius.html>_
