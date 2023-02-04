
### wgrib2: -var, -ext\_name, -set\_ext\_name



### Introduction


 In the beginning, grib fields were identified by a name, a level
and some timing information. Life was simple and the people were
happy. This was soon to pass, for the ensemble people had to 
specify the ensemble number and the probability of events.
The dust people needed to specify dust density by 
composition and by size. Thinking big, ensembles of dust
models were in the future albeit obscured by the haze.
Consequently the old name (ex. HGT, TMP) was often no longer
a good way to specify a specific field. For example, a
TMP field could be measured in degrees K or fraction
if the TMP had a probability modifier. The 
 -set\_ext\_name and -ext\_name
options are a way to help fix this problem. Using these
two options, you can get a extended name with many of the
modfiers added to the name. This option will have to be
updated when more modifiers are used to distiguish the fields.




The -var option prints the VARIABLE name of
the grib message. Common names would be HGT and TMP for the geopotential
height and the temperature. For most knowing the variable name,
the level and the timimg information is all you need. Then things
became more complicated. Eventually a file came along which had only
one variable type (MASSDEN, mass density) but had a couple of
important qualifier chemical type (H2O/O3/N02) and ensemble member ID.
The -AAIG output was useless because its output
used the variable name.


To fix the -AAIG output, an extended name
was introduced. You can see the extended name by 
the -ext\_name option.



```

-sh-2.05b$ ./wgrib2 chem.grb2 
1:0:d=2009012600:MASSDEN:surface:anl:ENS=hi-res ctl chemical=Water Vapour
-sh-2.05b$ ./wgrib2 chem.grb2 -ext\_name
1:0:MASSDEN.hi-res_ctl.Water_Vapour
-sh-2.05b$ ./wgrib2 chem.grb2 -misc 
1:0:ENS=hi-res ctl:chemical=Water Vapour

```


The extended name takes the output of -misc,
changes the colons to periods, spaces to underscores and removes the
text up to the equal size and appends it to the variable name. As of
wgrib2 v1.9.0, the extended name is used with 
-AAIGc, -csv, and -netcdf.
To stop using the extended name in -AAIG, -csv and -netcdf, use the option
-set\_ext\_name 0.



### Usage



```

-ext_var
-set_ext_name 0/1
-var

```

### Examples



```

-sh-2.05b$ ./wgrib2 chem.grb2 -var
1:0:MASSDEN
-sh-2.05b$ ./wgrib2 chem.grb2 -ext\_name
1:0:MASSDEN.hi-res_ctl.Water_Vapour

```




See also: 
[-s](./s.html),
[-varX](./varX.html)








----

>Description: inv          short variable name

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/var.html>_