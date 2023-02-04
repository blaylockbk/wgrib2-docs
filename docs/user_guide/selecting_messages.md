# wgrib2: selecting messages

## Introduction

Grib files contain many grib messages (records, fields). Often you only want to
process selected fields. With wgrib, you could process all "-d all", specific
message number, "-d N", or by using "grep" and the "-i" option.

```

   wgrib -d all grb                            writes all records to binary file to "dump"
   wgrib -d 10  grb                            write record 10 to binary file "dump"
   wgrib grb | grep ":HGT:" | wgrib -i grb     write HGT fields to "dump"

```

Wgrib2 is very similar. The differences is that "-d all" is gone and writing
to binary file is not a default option.

```

   wgrib2 -bin dump grb                       write all records to binary file "dump"
   wgrib2 -d 10   -bin dump grb               write record 10 to binary file "dump"
   wgrib2 -d 10.1 -bin dump grb               write record 10.1 to binary file "dump"
   wgrib2 grb | grep ":HGT:" | wgrib2 -i grb -bin dump    write HGT fields to "dump"
   wgrib2 -match ":HGT:" grb -bin dump        new syntax: write HGT fields to "dump"

```

One difference is record numbers may have a "decimal point". With grib2, for example,
the U and V winds may be in the same grib2 record. In the wgrib2 notation,
the first submessage would be N.1 (or N) and the second submessage would be N.2.

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
Page last modified: May 15, 2005
| [Disclaimer](https://weather.gov/disclaimer.php) | [Privacy Policy](https://weather.gov/privacy.php) |

|

---

> Description: selecting messages

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/selecting_messages.html>_
