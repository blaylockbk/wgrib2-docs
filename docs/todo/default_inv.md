# wgrib2: default inventory format

## Introduction

One of wgrib2 functions is to inventory grib2 files. The inventory
format is very customizable; at present there are 120 options
to modify the inventory. However the default inventory (-s)
is most often used and understanding this format will help you to
understand the structure of the inventory.

### Format

The format for the wgrib2 invetory is relatively simple. There is one line
for each gridded field. Usually each grid is in its own grib message
or record. However, is is possible to store related grids in into
a single grib message. Below are some examples.

```
Example 1:  1:4:d=2009060500:RH:2 m above ground:330 hour fcst:std dev
Example 2:  1:0:d=2010032900:TMP:2 m above ground:anl:
Example 3:  1:40:d=2011062206:ICAHT:cumulonimbus base:6 hour fcst:
Example 4:  1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
Example 5:  34.1:1464607:d=2009060500:UGRD:10 m above ground:180 hour fcst:ENS=+19
            34.2:1464607:d=2009060500:VGRD:10 m above ground:180 hour fcst:ENS=+19

Output Format:

  (msg|msg.submsg):(byte location):d=(YYYYMMDDHH):(variable):(level):(ftime):(optional attr)

  msg = message/record number starting from 1
  submsg = submessage number starting from 1 (optional)
  byte location: byte location of the start of the message starting from 0
  YYYYMMDDHH = reference time of the grib message, YYYY = year, MM = month (01..12), DD = day, HH = hour
  level = level/layer attributes, ex. "500 mb", "ground", "2 m above ground"
  ftime = time attributes, ex. "12 hour fcst", "anl", "0-6 hr ave fcst"
  optional attr: optional attributes that are needed to define the field such as ensemble memeber,
                 chemical type (for concentration), probability range
```

### NOMADS: point value (text)

The Nomads g2subset/grib-filter software can download point-value text files.
These text files are simply wgrib2 inventories created with the following options.

```
   wgrib2 IN.grb -crlf -v -s v0 -start_ft -lon (LON) (LAT)

   -crlf adds a cr-lf at the end of each line instead of the default lf or newline.
          (for windows users)
   -v turn on verbose mode
   -s is the standard inventory in verbose mode
   -v0 turn off verbose mode
   -start_ft adds the forecast or verification time or the start of the verification time
             if a forecast for a period of time (ex. 3-4 month forecast).
   -lon adds the point value to the inventory
```

```
Output Format:

  (msg|msg.submsg):(byte location):d=(YYYYMMDDHH):(verbose variable):(level):(ftime):(optional attr):
         start_ft=(YYYYMMDDHH):lon=(lon),lat=(lat),val=(value)

  msg = message/record number starting from 1
  submsg = submessage number starting from 1 (optional)
  byte location: byte location of the start of the message starting from 0
  YYYYMMDDHH = reference time of the grib message, YYYY = year, MM = month (01..12), DD = day, HH = hour
  verbose variable = variable name, description and units
  level = level/layer attributes, ex. "500 mb", "ground", "2 m above ground"
  ftime = time attributes, ex. "12 hour fcst", "anl", "0-6 hr ave fcst"
  optional attr: optional attributes that are needed to define the field such as ensemble memeber,
                 chemical type (for concentration), probability range
  start_ft = start of the forecast/verification time
  val = value of the grid point at (lat) and (lon).
```

In example 1, the optional attributes is "std dev". In this case, the field is the standard deviation
of an ensemble. The field cannot be a temporal std dev because the ftime field is "300 hour fcst".
(A temporal std dev would have been encoded in the ftime field.).

In example 5, we have the first field beind 34.1 and 34.2. In this example, message number 34 has
two submessages. Notice that the byte location is the same for both sub messages.

The inventory consists of one line per field. The first two fields (record number and byte location) are
fixed and the other fields will depend on the inventory options that are used. For example, the
default option (-s) is a macro that runs the -t, -var, -lev -ftime and -misc options.

In the future, the date codes in the nomads point-value text files may be extended to include minutes.

The default option gives the starting but not the ending location of each grib message. The ending
location is one less than the starting location of the next grib message (assuming no junk bytes).
The -range option will print out the starting and ending location of each record. Using the range,
a grib message can be extracted from a grib file. However, submessages are different. It is much
more difficult to extract a submessage from a grib message.

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
Page last modified: May 15, 2005
| [Disclaimer](https://weather.gov/disclaimer.php) | [Privacy Policy](https://weather.gov/privacy.php) |

|

---

> Description: Default Inventory Format

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/default_inv.html>_
