# wgrib2: -netcdf

## Introduction

The -netcdf option writes the grid values to a specified
file in netcdf format using COARDS convention for the Latitude-Longitude, Mercator
and Gaussian grids and the CF-1.0 convention for Lambert and Polar stereographic
projection grids. Other grid templates and projections are not supported yet.

Default the grid values are written as 4 bytes float having {TIME,LAT,LON}
dimension shape (3D data) and possible vertical LEV information added to the name of variables.
For example, V wind component defined at 10 m level in atmosphere will have default name
VGRD_10maboveground.
With some minimal efforts and using the sub-options described below it is also possible to write data as a 4D data
with the {TIME,LEV,LAT,LON} dimension shape that could be usefull for the vertical
structure analysis etc.

3D and 4D data could be mixed in the same netcdf file if both have the same horizontal
grid shapes. Practically any number of variables could be placed in to the single netcdf file
as well as time dimension can be almost arbitrary extended. For these purposes
please use the wgrib2 -append option.
All missing values in data are replaced by the \_FillValue defined in wgrib2 as
9.999e+20 for the float data, 32767 for short-packed data and 127 for byte-packed data.

### Simple usage 1

```

-netcdf file_name

```

### Example 1

```

$ wgrib2 ../example/eta.t00z.awphys18.grb2 -netcdf eta.nc

The above line converts the grib2 file into a netcdf file.

```

### Simple usage 2

```

-set_ext_name 1 -netcdf file_name

```

The first simple usage was good for simple variables like HGT (geopotential height)
and TMP (temperature). Using the grib name identifies the field. However,
some grib fields cannot be identified by the grib name. For example,

```

$ wgrib2 massmr.grb2 -netcdf junk\_simple\_name.nc
1:0:d=2015072700:MASSMR:64 hybrid level:12 hour fcst:aerosol=Dust Dry:aerosol_size >=2e-07,<2e-06:
      The grib name is MASSMR (mass mixing ratio.  If you make a netcdf file with
      this name, you get an ambigous netcdf field.
$ ncdump -h junk\_simple\_name.nc
...
	float MASSMR_64hybridlevel(time, latitude, longitude) ;
		MASSMR_64hybridlevel:_FillValue = 9.999e+20f ;
		MASSMR_64hybridlevel:short_name = "MASSMR_64hybridlevel" ;
		MASSMR_64hybridlevel:long_name = "Mass Mixing Ratio (Mass Fraction in Air)" ;
		MASSMR_64hybridlevel:level = "64 hybrid level" ;
		MASSMR_64hybridlevel:units = "kg/kg" ;

```

The netcdf file, junk_simple_name.nc, contains the mass missing ratio of some unknown substance.
For this case we need to use the extend name.

### Example 2

```

$ wgrib2 massmr.grb2 -set\_ext\_name 1 -netcdf junk\_ext\_name.nc
1:0:d=2015072700:MASSMR.aerosol=Dust_Dry.aerosol_size_>=2e-07,<2e-06.:64 hybrid level:12 hour fcst:
$ ncdump -h junk\_ext\_name.nc
...
	float MASSMR_aerosol_EQ_Dust_Dry_aerosol_size_GE_2eM07_LT_2eM06_64hybridlevel(time, latitude, longitude) ;
		MASSMR_aerosol_EQ_Dust_Dry_aerosol_size_GE_2eM07_LT_2eM06_64hybridlevel:_FillValue = 9.999e+20f ;
		MASSMR_aerosol_EQ_Dust_Dry_aerosol_size_GE_2eM07_LT_2eM06_64hybridlevel:short_name = "MASSMR_aerosol_EQ_Dust_Dry_aerosol_size_GE_2eM07_LT_2eM06_64hybridlevel" ;
		MASSMR_aerosol_EQ_Dust_Dry_aerosol_size_GE_2eM07_LT_2eM06_64hybridlevel:long_name = "Mass Mixing Ratio (Mass Fraction in Air)" ;
		MASSMR_aerosol_EQ_Dust_Dry_aerosol_size_GE_2eM07_LT_2eM06_64hybridlevel:level = "64 hybrid level" ;
		MASSMR_aerosol_EQ_Dust_Dry_aerosol_size_GE_2eM07_LT_2eM06_64hybridlevel:units = "kg/kg" ;

```

### Sub-options for -netcdf

Next sub-options could help to customize created netcdf file(s).
All sub-options must preceed the -netcdf option as it is the wgrib2 rule.
More then one -netcdf option could be given on the wgrib2 command line,
please read the documentation for wgrib2.
Almost all sub-options have -no\_... version that annulates previous
assignment.
For the advanced users it is recommended to use the GRIB2 to NETCDF conversion table
with the -nc_table sub-option or combine the command line options
with instructions in the -nc_table conversion file.

The -append option makes possible to create single
large data set for long time series of gridded data. Time dimension of data in the netcdf file
is "unlimited", it could be extended "forward" when adding new data but not "backward" before
the first defined time value. If data for the same time step already exist in the updated
netcdf file these will be silently overwritten.

The -nc_nlev option is needed to export data to netcdf
as 4D data defined in {TIME,LEV,LAT,LON} space.
It must be followed by integer max_number_of_vertical_levels
which defines the vertical dimension size of 4D data exported to the netcdf file.
This value can not be found from each single grib2 message or sub-message,
usually it has to be scanned the entire grib2 file or even number of files.
It is the reason why the max_number_of_vertical_levels
value must be provided by user.

Grib2 types of vertical levels eligible for export to the netcdf as 4D data
are defined in the wgrib2 internal table and now include next types of GRIB2 levels:

```



|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 20 : K level
| 100 : pressure level
| 104 : sigma level
| 105 : hybrid level
| 107 : K isentropic level
| 160 : ocean depth below sea level
 | | |
 | | |
 | | |
 | | |
 | | |
 | | |



```

Data for the first found eligible level type are treated as 4D and vertical level
information is not added to the variable names.
Error is generetad if data for other then first found eligible level type
are met in the input stream.
Data on the non-eligible levels are treated as 3D data defined in the {TIME,LAT,LON} space
with the possible level information included into the variable names or with the user-specified names
(see -nc_table option description below for how to do it).

The -nc_nlev option has not -no\_... option,
please use instead the max_number_of_vertical_levels equal to 0.
In this case all data from the input stream will be threated as 3D.

When existing netcdf file is updated in the -append mode
the value of max_number_of_vertical_levels must not exceed
initial value provided when the netcdf file was first created (defined).
By default when first creating the netcdf file, vertical level values are not fixed
(are undefined) and these are defined one-by-one when data at new level are added
to the netcdf file, up to the max_number_of_vertical_levels.
To overcome this feature please use the -nc_table option.

The -nc_pack option makes it possible to limit the range of defined values and pack data in the netcdf file specifying data range and packing type as min:max[:float|byte|short]. Type float here is
default and do not assume data packing, only data range is checked. Data outside of
specified range are replaced by missing value code for all types of packing.
This replacement is done in the local copy of unpacked data, so main data array
stays unchanged.
Values of min and max could be any
signed float. Packing is applied to all new variables in the input stream.
If some variable was already defined in the netcdf file and now is appended to it
in the -append mode the initially defined and fixed in the netcdf file
valid range and packing parameters are used.

The packing is possible to the short (2 bytes)
or byte (1 byte) values with potential loss of precision.
Both zero values of min and max
packing parameters for the short or
byte packing activate 'auto' packing when
min and max
values are defined from the first entered field.

PS: packing in 'byte' is not directly accepted by GrADS v1.9b4 if netcdf file
is open with 'sdfopen' command. But by some reasons using the GrADS
data description file (catalog) for the same netcdf file helps in this situation.

The -nc_grads option activates some tests for the created
netcdf file to be GrADS (version 1.9b4) compatible,
or to be open by sdfopen in gradsnc or gradsdods.
The GrADS support only COARDS convention netcdf files;
it do not support non-constant data time stepping
and silently generates wrong time stamps for such netcdf files.
Packing to byte also is not directly recognized by GrADS v1.9b4.
With this option an error is raised and processing stops
if any of criterias above is determined.

The -nc*table option followed by the \_file_name* is most usefull for the advanced users
as it specifies the file where user can customize many features of created netcdf file.
Next examples demonstrate which directives and conversion rules could be given
in the -nc_table file.
Almost all of them are optional with exception of $nlev for the case
when user explicitly specifies the vertical level values $levs.

$lev_type 100:pressure:pressure level:mb:0.01
This directive explicitly defines the type of vertical level
in the grib2 file that becomes eligible for treating data at these levels as 4D data
and some it attributes for netcdf file.
Default, first found level type listed in the wgrib2 build-in table
of eligible levels is selected if no $lev_type directive found.
Fields here include:

```



|  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 100 - the grib2 level type code number
for treating data as 4D. In this example it is the code number for pressure levels, integer
| pressure -
short\_name for the vertical axis in the netcdf file, string
| pressure level - long\_name for the vertical axis in the netcdf file, string, could include spaces
| mb - vertical axis units as written in the netcdf file, string
| 0.01 - decimal scale to convert grib2 level values to the netcdf-stored values, float. In this example we convert Pa to mb (hPa). It is default conversion scale for the pressure levels when exporting to the netcdf file
 | | |
 | | |
 | | |
 | | |
 | | |



```

$nlev X
This directive is an equivalent of the -nc_nlev command line option
but value in the -nc_table file has precedence over the command line option if both are found.
It is required if next $levs directive is specified.

|                               |                                |                    |
| ----------------------------- | ------------------------------ | ------------------ |
| $levs lev(1) lev(2) lev(3)... | ...lev(I-1) lev(I) lev(I+1)... | ...lev(X-1) lev(X) |
|                               |
|                               |

This directive explicitly specifies the vertical level values to be exported
to the netcdf file, in the netcdf units.
If this directive exists in the -nc_table file and there is found the grib2 data
eligible for the 4D presentation but defined on other then listed level -
these data are skipped from export and work is silently continued.
All data conversion parameters are checked before the $levs is checked.
List of level values could consist of multiple lines;
max line length in the -nc_table file is limited by \_MAX_PATH symbols (about 255 symbols);
use space, ',', ';' or ':' as lev(i) fields separator.

Impact of $nlev and $levs directives is different depending on
does the netcdf file is first created or it is updated.
When the netcdf file is first created these values are written
to the netcdf file fixing 4D data vertical structure.
When updating an existing netcdf file in the -append mode
these parameters will work like filter limiting possible
updates of 4D data by these that satisfy to the given $levs only and
ignoring other. 3D (TIME,LAT,LON) data are not affected by these directives.

The $nlev and $levs directives could be necessary if records
in the grib2 file have randomly changing vertical level values
that makes it impossible sequentionally define valid order of vertical levels.

$grads 1
This directive is an equivalent of the -nc_grads command line option
if passed value is 1 or to the -no_nc_grads if value is 0.
Directive in the -nc_table file has precedence over the command line option.

wgib2_name:wgrib2_level|\*:nc_name|ignore[:ignore|no|float|short|byte[:min:max]]

All non-empty lines in the -nc_table file not starting from the '$' symbol or from the comment mark '#'
are treated as grib2 to netcdf conversion rules for the specified variables.
In these strings the wgib2_name and wgrib2_level
are strings as returned by the wgrib2 inventory; \* used as the wgrib2_level will apply
to all levels not given explicitly.
The valid range and packing information is optional but it overwrites common
packing rule if such is specified by the -nc_pack command line option.
Absence of packing information means no packing for this variable.
Min and max values are any signed float values. Both could be omitted or put to zero.
Last case means that automatic scaling will be estimated from the first entered
wgib2_name field at the wgrib2_level
or at the first level in case of \* as level value.

If the keyword ignore is found as a netcdf variable name or
as a packing type value, the corresponding data are ignored
and do not written to the netcdf file.
Impact from this keyword is similar to the wgrib2 -not option
or filtering data with the grep utility.

The ignore keyword is recommended if the data from the same
grib2 file are exported in number of output files (netcdf or other)
by the same wgrib2 process. Then the same decoded data could be passed
for the output in other file of any supported type.
Doing export to the single netcdf file it is not recommended to use the
ignore keyword
as corresponding data are first decoded and after that skipped from writing
to the netcdf file.

In the next example two lines from the -nc_table file instruct
the wgrib2 utility export to the netcdf file the geopotential height changing name
from HGT (wgrib2) to
geopotential in the netcdf file,
at all levels except at 975 mb if these data would be found in the grib2 decoded input data stream:

```


HGT:\*:geopotential
HGT:975 mb:ignore


```

All other variables not listed in the -nc_table file but found in the input stream
are processed as regular 3D data.

### Example 3

Next example shows the content of user-defined -nc_table file
for one special case of Japan Meteorological Agency MSM system grib2 data conversion.
Same -nc_table file includes the rules for
conversion of both 'surface' and upper air data although
the input grib2 files are different.

```


#
# File name: jma\_msm\_g2nc.table
# Description: Specifies parameters for conversion to Netcdf format
# hourly analyses and forecasts of JMA MSM system as best suited to
# my purposes: forcing of ocean model and reference data for analysis
# of meteorological conditions with GrADS-based visualization tools.
# Sergey Varlamov, July 2007
#
$lev\_type 100:p:pressure level:mb:0.01
$nlev 16
$levs 1000 975 950 925 900 850 800 700 600 500
 400 300 250 200 150 100
$grads 1
#
# Upper air data on pressure levels, 05.2007 - 16 pressure levels
#
UGRD:\*:u
VGRD:\*:v
#UGRD:\*:u:ignore
#VGRD:\*:v:ignore
TMP:\*:temp
#write HGT at any level as hgtprs:
HGT:\*:hgtprs
#but skip HGT at 100 mb, now commented - example and test
#HGT:100 mb:hgtprs:ignore
RH:\*:rh:short:-2:110
VVEL:\*:omega
#
# JMA MSM model surface data conversion rules
#
PRMSL:mean sea level:prmsl
PRES:surface:prsfc
UGRD:10 m above ground:u10m
VGRD:10 m above ground:v10m
TMP:1.5 m above ground:t2m
RH:1.5 m above ground:rh2m:short:-5:110
TCDC:surface:ncld:short:-5:110
LCDC:surface:ncld\_low:short:-5:110
MCDC:surface:ncld\_mid:short:-5:110
HCDC:surface:ncld\_upper:short:-5:110
APCP:surface:r1h
# End of file


Using above file we could create the daily file with 3h analyses
at the 'surface' layers from the input grib2 files
with the next simple shell script:


date=20070715
for (( j=0; j<=21; j+=3 )) #hour of day loop
do
 if [ $j -lt 10 ]
 then
 h=0$j
 else
 h=$j
 fi
 gpath=../example/Z\_\_C\_RJTD\_${date}${h}0000\_MSM\_GPV\_Rjp\_Lsurf\_FH00-15\_grib2.bin

 wgrib2 $gpath -match ":anl:" -nc\_table ./jma\_msm\_g2nc.table -append -netcdf ./msm\_s\_${date}.nc
 code=$?
 if [ $code -eq 0 ]
 then
 echo ${date}${h}': created/added to Netcdf'
 else
 echo ${date}${h}': wgrib2 failed...'
 exit $code
 fi
done


Or create the netcdf file with the 4D 3h analyses at 16 pressure levels if replace:

gpath=../example/Z\_\_C\_RJTD\_${date}${h}0000\_MSM\_GPV\_Rjp\_L-pall\_FH00-15\_grib2.bin

and change the name for the output file:

./msm\_s\_${date}.nc to the ./msm\_p\_${date}.nc

Resulting netcdf files could be used directly at least from the GrADS (gradsnc or gradsdods)
utility for data visualization and analyses.

```

### Example 4

In this example, we want to take some grib files, extract the 200, 500 and 700 height fields,
convert to a regional subset and finally convert it into a netcdf file. Since the -netcdf
option doesn't support appending, the netcdf conversion cannot be in a loop.

```

Version 1, using loops and a temporary file

rm tempfile
for f in gfs.t00z.pgrb2f[01]?
do
   wgrib2 - -match ':HGT:' -match ':(200|500|700) mb:'  -append -new_grid_winds earth -new_grid latlon 0:40:2 20:20:2 tempfile
done
wgrib2 tempfile -netcdf ~/myfile.nc
rm tempfile

Version 2, data-flow

1:   cat gfs.t00z.pgrb2f[01]? | \
2:   wgrib2 - -match ':HGT:' -match ':(200|500|700) mb:'  -inv /dev/null \
3:      -new_grid_winds earth -new_grid latlon 0:40:2 20:20:2 - | \
4:   wgrib2 - -netcdf ~/myfile.nc


Line 1 (cat), takes all the desired grib files and writes to stdout
Line 2 (wgrib2), reads the grib file from stdin and selects the desired fields
Line 3 (-new_grid), is a continuation of the wgrib2 command and writes the new grid to stdout
Line 4 (wgrib2), reads the new grid from stdin and writes out a netcdf file (myfile.nc)

```

### Problems

The -netcdf option uses the "verification" time (-vt) as
the time coordinate in the netcdf file. Unlike in grib, the definition of
new "time" values has to go chronologically. For example, the analysis,
6-hour forecast and then the 12-hour forecast. Allocation of new time values
in a different order will result in error messages. If your grib file
doesn't follow the order, you can sort the file by the verification time.

```

wgrib2 FILE.old -vt -s | sort -k3,3 -t: | wgrib2 -i FILE.old -grib FILE.new
wgrib2 FILE.new -netcdf FILE.nc

```

Another problem is that netcdf-3 files are limited to 4GB in size. The
grib2 format supports compression so that conversion to uncompressed netcdf3 or
the poorly compressed netcdf4 can increase the file size dramatically.
I am sure that Western Digital, Seagate, Hitachi and Toshiba all appreciate
your support.

See also:
[-bin](./bin.html),
[-ieee](./ieee.html),
[-text](./text.html),
[-set_ext_name](./set_ext_name.html),
[-spread](./spread.html),

[-append](./append.html)

---

> Description: out X write netcdf data to X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/netcdf.html>_
