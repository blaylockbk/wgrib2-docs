
### wgrib2: -set\_ensm\_derived\_fcst



### Introduction: set ensemble mean derived forecast



The -set\_ensm\_derived\_fcst option changes 
Product Definition Template (PDT) 0,1,2 -> 2 or 8,11,12 -> 12. PDT 2, and 12 
are for "ensemble mean derived forecasts". Effectively this options describes the forecast
as being derived from an ensemble of forecasts. The types derived forecasts is listed in
[code table 4.7](https://www.nco.ncep.noaa.gov/pmb/docs/grib2/grib2_table4-7.shtml).
The "cluster" derived forecast should not be used with this option as PDT 2 and 12 do not
have metadata to describe the cluster that was used.


### Usage




```

-set_ensm_derived_fcst X Y
X = [code table 4.7](https://www.nco.ncep.noaa.gov/pmb/docs/grib2/grib2_table4-7.shtml)
Y = 0..255  number of forecasts in the ensemble

```

### Example



```

$ wwgrib2 sst.grb2 -set\_ensm\_derived\_fcst 1 10 -grib newsst.grb
1:0:d=1981110100:TMP:surface:0-1 month ave anl:wt ens-mean

-set_ensm_derived_fcst 1 10
    1 = weighted mean of all members (code table 4.7)
   10 = number of ensemble members

-grib newsst.grb
   save the new grib file in newsst.grb

```



See also: 
[-set\_ens\_num](set_ens_num.html),
[-ens](ens.html),
[-N\_ens](N_ens.html)






----

>Description: misc  X Y    convert PDT 0,1,2 -> 2, 8,11,12 -> 12, X=code table 4.7 Y=num ens members

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_ensm_derived_fcst.html>_