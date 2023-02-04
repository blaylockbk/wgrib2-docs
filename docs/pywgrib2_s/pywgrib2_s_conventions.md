# pywgrib2_s: conventions

### Conventions

What is a convention? In the year 2020, a convention is a gathering to
decide who runs for political office. In bridge, a convention is an
understanding that certain artifical bids have special meanings. For example,
a opening 4 no trump bid asks the partner to disclose the number aces in his hand.
The pywgrib2_s conventions are more like the bridge conventions.

### Optional Parameter name starts with an uppercase

The optional parameter expects a logical value, and the default value is False.

### Optional Parameter has a default value of "" (empty string)

The optional parameter expects a string value, and an empty string turns off
the option.

### Optional Parameter has a default value of None

The optional parameter expects something besides a string or logical value.
It may exoect a number, a numpy array, or mutiple types of input.

### Undefined Grid Values

Most reasonable computers have floating values that can be NaN (not a number).
Using NaN can be a problem because "if (x == NaN)" is not the way to test
if x is a NaN. Pywgrib2_s uses the numpy.nan for the NaN. Some codes may
be based on wgrib2 scripts which use 9.999e20 as the value for undefined grid point value.
You can change pwgrib2_s to use the wgrib2 convention by setting pywgrib2\_.s.use_numpy.nan to False.
Note that numpy.nan is not the same as math.nan; for example (math.nan is numpy.nan) is false.

### Numpy Arrays

The grid values, lat and lon arrays are returned as numpy arrays. The arrays
are in Fortran order, and in WE:SN order. In english,

- a[i+1,j] is stored after a[i,j] in memory
- for a lat-lon grid: a[i,j] and a[i+1,j] have the same latitude
- for a lat-lon grid: a[i+1,j] is east of a[i,j] by delta-longitude degrees
- for a lat-lon grid: a[i,j] and a[i,j+1] have the same longitude
- for a lat-lon grid: a[i,j+1] is north of a[i,j+1] by delta-latitude degrees

### Debugging

Change the global variable pywgrib2_s.debug to True.

---

> Description: Conventions

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pywgrib2_s_conventions.html>_
