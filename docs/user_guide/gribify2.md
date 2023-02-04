### gribify using import and a template

## Introduction

Here is another way of writing a grib2 file. Create a template
grib2 file. In our example, we already created one in grib1 and then
converted it to grib2 using cnvgrib. Once we have a template file, we can
replace the data using one of the import options, adjust the
date code, handle the undefined grid point and write it out using
the -grib_out option. Here is
an example script.

```
#!/bin/sh
#
# example of converting a binary file to grib2
#
# conustemplate.grb2 is the template
#  was created in grib1 using the gribw library
#  and converted to grib2 by cnvgrib
#
# the data file has two records but we only want to convert the 1st record
#
set -x

# date=$1
# date0=`ndate -12 ${date}00`
date=20090414
date=2009041312

# -no_header    .. read/write data without f77 header
# -import_bin   .. read one binary record (no header) and replace the grib data field
# -set_date     .. set a new reference time
# -undefine_val .. the binary file used -999 as the undefined flag
# -rpn          .. use the rpn calculator to convert to standard units (x=x/10/86400)
# -grib_out     .. write the data as a grib file

wgrib2 conustemplate.grb2 -no_header -import_bin PRCP_CU_GAUGE_V1.0CONUS_0.25deg.lnx.$date.RT \
  -set_date $date0 -undefine_val -999 -rpn 10:/:86400:/ -grib_out precip$date0.grb2
```

---

> Description: gribify using import and a template

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/gribify2.html>_
