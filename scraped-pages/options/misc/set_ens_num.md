
### wgrib2: -set\_ens\_num



### Introduction



The -set\_ens\_num option changes PDT 0,1 -> 1 and
8,11 -> 11. PDT 1 and 11 are for "forecasts from a specified member of an ensemble.
Effectively this option add an "ensemble member ID" to the forecast.

 With wgrib2 v3.1.1, more PDTs can be promoted to ensemble-member PDTs.

### Merging ensembles, Adding New Members


 When ensembles are made, each member has a unique perturbation/member 
number. When you want to merge ensembles, the member numbers
may no longer be unique. You would want to renumber the forecasts
sp that they would be unique. Sometimes you would want to add a forecast
with no ensemble information; you need a way to add a ensemble member number.


### Usage




```

-set_ens_num X Y Z
X = 0..255 type of ensemble member (Code Table 4.6)
Y = 0..255 perturbation number
Z = 0..255  number of forecasts in the ensemble

```

### Example



```

$ wgrib2 ds.td.bin -set\_ens\_num 3 1 10 -grib ds\_ens.grb
1:80:d=2009062918:DPT:surface:60 hour fcst:ENS=+1
2:218826:d=2009062918:DPT:surface:66 hour fcst:ENS=+1
3:434430:d=2009062918:DPT:surface:72 hour fcst:ENS=+1
4:652869:d=2009062918:DPT:surface:78 hour fcst:ENS=+1
5:871866:d=2009062918:DPT:surface:84 hour fcst:ENS=+1
6:1088694:d=2009062918:DPT:surface:90 hour fcst:ENS=+1
7:1304003:d=2009062918:DPT:surface:96 hour fcst:ENS=+1
8:1549304:d=2009062918:DPT:surface:102 hour fcst:ENS=+1
...

-set_ens_num 3 1 10
    3 = positive perturbed forecast
    1 = perturbation number
   10 = number of ensemble members

-grib ds_ens.grb
   save the new grib file in ds_ens.grb

```



See also: 
[-set\_ensm\_derived\_fcst](set_ensm_derived_fcst.html),
[-ens](ens.html),
[-N\_ens](N_ens.html)










----

>Description: misc  X Y Z  ensemble member info, X=code table 4.6 Y=pert num Z=num ens members -1=No Change

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_ens_num.html>_