# wgrib2: -fix_CFSv2_fcst

## Introduction

The Climate Forecast System version 2 (CFSv2) is a coupled atmosphere/ocean
forecast system that was developed at NCEP. CFSv2 produces analyses (CFSR)
and daily to seasonal forecats. The seasonal forecasts consists of hindcasts
(CFSRR) and the real-time forecasts (CFSv2 forecasts).

The monthly-mean forecasts from the CFSRR and CFSv2 are originally
written in grib1 format and later converted to grib2 format for public
distribution. In this process, the resulting grib2 files are mangled
and the metadata no longer correctly describes the contents of the file.
The -fix_CFSv2_fcst option alters the metadata of the
CFSRR/CFSv2 monthly forecasts files to make them grib2 compliant and to
fix the metadata. After the data has been fixed, the files can
then be read with grib2 decoders such as GrADS.

### CFSRR/CFSv2 Monthly Forecast File Names

The CFSv2 monthly forecasts have a naming convengtiion.

```
(NAME)(DATE0).(EN).(DATE1).avrg[.(HH)Z].grb2

NAME=type of file
     flxf       forecast (f) data on special levels (flx)
     ipvf       forecast (f) data on isentropic surfaces (ipv)
     ocnf       forecast (f) data for the oceans (ocn)
     pgbf       forecast (f) data for pressure (p) levels grib (gb)

DATE0=starting time of the forecasts
      YYYYMMDDHH

EN=ensemble member number

DATE1=target month of the forecast
      YYYYMM

[..] = contents in the square brackets are optional

Case 1: contents of [] are missing or null
        The forecast system produces daily forecasts ending at 00Z, 06Z, 12Z and 18Z.
        All these forecasts are included in the forecast of the monthly mean.

Case 2: contents of [] are present, i.e., .(HH)Z
       HH = 00, 06, 12, 18
       The monthly forecast is for a specific cycle of the day.
       For example the 00Z cycle.  For instantaneous quantities,
       the monthly forecast will be consist of the mean of all daily forecasts
       that verify on the 00Z hour.  For quantities that are 6 hour
       averages or accumulations, they will be the average of all the
       forecasts with averages/accumulations that end at 00Z hour
       (18Z-24Z average/accumulations).

       Unfortunately the original file doesn't contain any metadata
       concerning the hour or the type of quantity, so the hour
       has to be entered as an argument and the forecasts are
       assumed to be the average of instantaneous fields.
```

### Fixing the CFSRR/CFSv2 monthly forecasts format

A filter was added to wgrib2 that fixes the metadata in
the CFSv2 monthly forecasts. To fix the metadata,

```
wgrib2 IN -fix\_CFSv2\_fcst TIME EN N\_ENS -grib OUT

IN=name of input grib file, ex. flxf2009010100.01.200901.avrg.06Z.grb2

TIME=daily, 00, 06, 12, 18
    use daily if .(HH)Z is missing from the input filename
    use 00 if .00Z is part of the input filename
    use 06 if .06Z is part of the input filename
    use 12 if .12Z is part of the input filename
    use 18 if .18Z is part of the input filename

EN=ensemble member number, suggested that EN from the filename is used but value is arbitrary.

N_ENS=number of ensemble members, use max of (EN).  Value is arbitrary.

OUT=name of the output grib file
```

### Example of Fixing the CFSRR monthly forecast

```
bash-3.2$ wgrib2 flxf2009010100.01.200903.avrg.06Z.grb2 -for 1:3 -fix\_CFSv2\_fcst 06 1 1 -grib out.grb
1:0:d=2009010100:UFLX:surface:1422 hour-(1422 hour+30 day) ave@(fcst,dt=1 day),missing=0:ENS=+1
2:66720:d=2009010100:VFLX:surface:1422 hour-(1422 hour+30 day) ave@(fcst,dt=1 day),missing=0:ENS=+1
3:133122:d=2009010100:SHTFL:surface:1422 hour-(1422 hour+30 day) ave@(fcst,dt=1 day),missing=0:ENS=+1

flxf2009010100.01.200903.avrg.06Z.grb2   input file
-for 1:3                                 process records 1..3
-fix_CFSv2_fcst 06 1 1                   fix metadata for 06Z type file,
-grib out.grb                            save corrected file in out.grb
```

### Examining our corrected file

The default wgrib2 inventory looks like

```
bash-3.2$ wgrib2 out.grb
1:0:d=2009010100:UFLX:surface:1422 hour-(1422 hour+30 day) ave@(23 hour fcst)++,missing=0:ENS=+1
2:66719:d=2009010100:VFLX:surface:1422 hour-(1422 hour+30 day) ave@(23 hour fcst)++,missing=0:ENS=+1
3:133120:d=2009010100:SHTFL:surface:1422 hour-(1422 hour+30 day) ave@(23 hour fcst)++,missing=0:ENS=+1

d=2009010100                             initial time of the forecast
UFLX                                     zonal wind stress
1422 hour-(1422 hour+30 day) ave         average for 1422 hour to (1422 hours + 30 day)
(fcst,dt=1 day)                          average the forecast at 1 day intervales
missing=0                                no missing fields in the average
```

Now I can't figure out 1422 hours in my head, so I can print out the start and and of the
forecast time interval by,

```
bash-3.2$ wgrib2 out.grb -start\_ft -end\_ft
1:0:start_ft=2009030106:end_ft=2009033106
2:66719:start_ft=2009030106:end_ft=2009033106
3:133120:start_ft=2009030106:end_ft=2009033106
```

Notice the start_ft has a 06 hour as you would have expected from the original file,
flxf2009010100.01.200903.avrg.06Z.grb2.

### Adding Ocean Metadata

In the NCDC archives, the DBSS variable has bad level information.
For example, the time series of N degree isotherm is not encoded correctly

```
bash-3.2$ wgrib2 dt5c.ensm.apr.cfsv2.data.grb2
1:0:d=1982040618:DBSS:0C ocean isotherm:0-1 month ave fcst:
2:42547:d=1982040618:DBSS:0C ocean isotherm:1-2 month ave fcst:
3:85611:d=1982040618:DBSS:0C ocean isotherm:2-3 month ave fcst:
4:128757:d=1982040618:DBSS:0C ocean isotherm:3-4 month ave fcst:
...

The level should be 5C ocean isotherm as indicated by the file name.
```

To fix this time series, you need to rewrite the level information.

```
bash-3.2$ wgrib2 dt5c.ensm.apr.cfsv2.data.grb2 -set\_lev "5C ocean isotherm" -grib dt5c.ensm.apr.cfsv2.data.grb2.new
1:0:d=1982040618:DBSS:5C ocean isotherm:0-1 month ave fcst:
2:42547:d=1982040618:DBSS:5C ocean isotherm:1-2 month ave fcst:
3:85611:d=1982040618:DBSS:5C ocean isotherm:2-3 month ave fcst:
4:128757:d=1982040618:DBSS:5C ocean isotherm:3-4 month ave fcst:
...
```

In monthly ocean forecasts @ NCDC, we can find bad level data. For example,
one file had bad level metadata for messages 216-222.

```
216:11232042:d=1999052200:DBSS:0C ocean isotherm:2-3 month ave fcst:
217:11278152:d=1999052200:DBSS:0C ocean isotherm:2-3 month ave fcst:
218:11322088:d=1999052200:DBSS:0C ocean isotherm:2-3 month ave fcst:
219:11360222:d=1999052200:DBSS:0C ocean isotherm:2-3 month ave fcst:
220:11392818:d=1999052200:DBSS:0C ocean isotherm:2-3 month ave fcst:
221:11419935:d=1999052200:DBSS:0C ocean isotherm:2-3 month ave fcst:
222:11440970:d=1999052200:DBSS:0C ocean isotherm:2-3 month ave fcst:
```

The following code snippet will fix the level data for the above file.

```
wgrib2 $f \
 -if "^216:" -set_lev "2.5C ocean isotherm"  -fi \
 -if "^217:" -set_lev "5C ocean isotherm"  -fi \
 -if "^218:" -set_lev "10C ocean isotherm"  -fi \
 -if "^219:" -set_lev "15C ocean isotherm"  -fi \
 -if "^220:" -set_lev "20C ocean isotherm"  -fi \
 -if "^221:" -set_lev "25C ocean isotherm"  -fi \
 -if "^222:" -set_lev "28C ocean isotherm"  -fi  \
 -grib $f.fixed
```

### Making GrADS control files

Once you have converted the files using the -fix_CFSv2_fcst option,
you have to use the -b option in g2ctl/gribmap to get the target times to line up correctly.  
The -b option sets the start of the target month as the GrADS time. If you use the default option,
you will get the end of the forecast period. For the ordinary forecast files, that will be
at 18Z of the last month. Definately not as friendly.

```
bash-3.2$ g2ctl -b out.grb >out.ctl
bash-3.2$ gribmap -b -i out.ctl
grib2map: scanning GRIB2 file: out.grb
grib2map: Writing out the index file
```

### A Real Example

The previous examples were the conversion, inventory and display of a single file.
Most of the time, people would want to examine the entire forecast run. In this
followring example, we look at a forecast run. Here we cannot include
the file with the target month which is the same as the starting month
because this file does not start on the 1st of the month like the other files.

```
CFSRR files:

    (do not include this file) -->   pgbf2009081500.01.200908.avrg.grb2
    pgbf2009081500.01.200909.avrg.grb2
    pgbf2009081500.01.200910.avrg.grb2
    pgbf2009081500.01.200911.avrg.grb2
```

Fixing the metadata in the bash shell on a linux box.

```
bash-3.2$ for f in pgbf2009081500.01.2009??.avrg.grb2
>do
> wgrib2 $f -fix\_CFSv2\_fcst\_daily 1 1 -grib $f.new >/dev/null
>done
fix_CFSRv2_fcst 524 fields fixed
fix_CFSRv2_fcst 524 fields fixed
fix_CFSRv2_fcst 524 fields fixed
```

Making the GrADS ctl and idx files. Note the -b options to both g2ctl and gribmap.
Note that gribmap v2 and grads v2 have to be used. Note the use of the template (%m2) in
the g2ctl line.

```
bash-3.2$ g2ctl -b pgbf2009081500.01.2009%m2.avrg.grb2.new >pgb.ctl
bash-3.2$ gribmap -b -i pgb.ctl
grib2map: scanning GRIB2 file: pgbf2009081500.01.200909.avrg.grb2.new
grib2map: scanning GRIB2 file: pgbf2009081500.01.200910.avrg.grb2.new
grib2map: scanning GRIB2 file: pgbf2009081500.01.200911.avrg.grb2.new
grib2map: reached end of files
grib2map: Writing out the index file
```

Running grads v2 with the new ctl/idx file.

```
bash-3.2$ grads

Grid Analysis and Display System (GrADS) Version 2.0.a9
Copyright (c) 1988-2010 by Brian Doty and the
Institute for Global Environment and Society (IGES)
GrADS comes with ABSOLUTELY NO WARRANTY
See file COPYRIGHT for more information

Config: v2.0.a9 little-endian readline printim grib2 netcdf hdf4-sds hdf5 opendap-grids,stn geotiff shapefile
Issue 'q config' command for more detailed configuration information
Landscape mode? ('n' for portrait):
GX Package Initialization: Size = 11 8.5
ga-> open pgb.ctl
Scanning description file:  pgb.ctl
Data file pgbf2009081500.01.2009%m2.avrg.grb2.new is open as file 1
LON set to 0 360
LAT set to -90 90
LEV set to 1000 1000
Time values set: 2009:8:15:0 2009:8:15:0
E set to 1 1
ga-> set lev 500
LEV set to 500 500
ga-> d hgtprs
Contouring: 4900 to 5900 interval 100
ga->
```

### Encoding the Ensemble Information

The -fix_CFSv2_fcst option adds enesemble information. The directions
suggest that you number the runs using the same numbers as suggested by the CFSv2 filename convention.
That will minimize confusion. However, you are allowed to give any ensemble number
up to 254. Usiog unique ensemble member numbers would be useful GrADS.

### Caveats

The filtering action of -fix_CFSv2_fcst assumes that the fields
that were averaged were instantaneous fields (ex. Z500 at a specific time). This
is not true as some fields are 6-hour accumulations or averages. Grib2 can describe
averages of accumulations/averages. However, this nuance was ignored.

The instructions suggest that you encode the CFSRR enemble member number as 1 to be consistent
with the CFSv2 file name convention. For CFSRR, there is only one ensemble member (01) which
by would be considered the high-resolution control run.

## Usage

```
-fix_CFSv2_fcst X Y Z
X=daily, 00, 06, 12, 18
Y=Ensemble member number (perturbation number)
Z=number of ensemble members
```

```

```

---

> Description: misc X Y Z fixes CFSv2 monthly fcst X=daily or 00/06/12/18 Y=pert no. Z=number ens fcsts v1.0

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/fix_CFSv2_fcst.html>_
