# wgrib2: time interpolation

## Introduction

Sometime we have to do time interpolation. We may have the May 1st analysis and
the May 3rd analysis but are missing the May 2nd analysis. Or we may hourly forecasts
and want to make forecasts every 15 minutes in order to make a smooth animation.

The following shell script reads the 2 hour forecast ($1), the 3 hour forecast ($2)
and writes the 2:15, 2:30 and 2:45 hour forecast to the output file ($3).
The type of grib packing is retained by the option "-set_grib_type same".
The scaling of the numbers is retained by the option "set_scaling same same".
The latter option requires wgrib2 v2.0.5.

```
#!/bin/sh
# use wgrib2 to interpolate to time interpolate two forecast files
#
# ASSUMPTION: THE TWO FORECAST FILES MUST HAVE THE RECORDS IN THE
# SAME ORDER
#
# Yes, I am shouting.
#
# $1 = 2 hour forecast
# $2 = 3 hour forecast
# $3 = output   (2:15, 2:30 and 2:45 hour forecasts)
#
# note: wgrib2 v2.0.5 is needed for the -set_scaling same same
#    for older versions of wgrib2, remove the "-set_scaling same same"
#    the output will be written in the default mode .. 12 bits
#
wgrib2=wgrib2
in1=$1
in2=$2
out=$3

a="2 hour fcst"
b1=0.75
c1=0.25
d1="135 min fcst"

b2=0.5
c2=0.5
d2="150 min fcst"

b3=0.25
c3=0.75
d3="165 min fcst"

$wgrib2 $in1 -rpn sto_1 -import_grib $in2 -rpn sto_2 -set_grib_type same \
  -if ":$a:" \
     -rpn "rcl_1:$b1:*:rcl_2:$c1:*:+" -set_ftime "$d1" -set_scaling same same -grib_out $out \
  -if ":$a:" \
     -rpn "rcl_1:$b2:*:rcl_2:$c2:*:+" -set_ftime "$d2" -set_scaling same same -grib_out $out \
  -if ":$a:" \
     -rpn "rcl_1:$b3:*:rcl_2:$c3:*:+" -set_ftime "$d3" -set_scaling same same -grib_out $out
```

The script requires the the 2 and 3 hour forecast have the fields in
the same order. Otherwise you may be subtracting HGT from the TMP which
would be silly. The script only processes the fields with an inventory
line of ":2 hour fcst:". You could add lines to process accumulations and
averages. Note that the "-if" is ended by an output option which happens to be "-grib_out".

The script preserves the packing method and the scaling of the 2 hour forecast.

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
[Climate Prediction Center Web Team](/comment-form.md)
Page last modified: July 27, 2016.
| [Disclaimer](https://weather.gov/disclaimer.php) | [Privacy Policy](https://weather.gov/privacy.php) |

|

---

> Description: Time interpolation

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/time_interpolation.html>_
