# wgrib2: -set_ext_name

## Introduction

In the beginning, grib fields were identified by a name, a level
and some timing information. Life was simple and the people were
happy. This was soon to pass. A hoard of ensemble members appeared
in the distance. A dust storm rolled in from the plains. The new
power plants started putting sulfar dioxide into the air. All this
had to added to the grib format. So names had to have modifiers,
the percentile of some particulates of some chemical between size
A and B microns. So wgrib2 added ["extended names"](./ext_name.md).
So you could
switch beween the old simpler names and the newer extended names.

### Extended Names

The extended name includes modifiers like the ensemble information,
dust information and probabilities. Optionally you can include
the level and forecast information (wgrib2 v3.0.2+). More details
are given [here](./ext_name.md).

## Usage

To select between the regular and extended name, you use

```
   -set_ens_name N

      N == 0                 default
      N == 1                 add misc terms like ensemble or probability
                               extension for wgrib2 v3.0.2+
      and(N,1) == 1          add misc terms like ensemble or probability
      and(N,2) == 2          add level information
      and(N,4) == 4          add forecast information
      ex N == 3              misc + level information
      ex N == 5              misc + forecast information
      ex N == 6              level information + forecast information
      ex N == 7              misc + level information + forecast information
      N > 7                  for future use
```

### Extended Extended Names

The extended name facility was extended with wgrib2 v3.0.2 in order
to fix a problem with converting grib to netcdf with some of the newer
NCEP files. When wgrib2 writes netcdf files, the -netcdf concatinates
the level information to the extended name to produce the netcdf name
for the field. For some of the newer NCEP forecast files, this name
wasn't unique. So the extended name needs to optionally include
the forecast time information. With this extension, the level
information was added as another option field to the extended name.

```
  $ wgrib2.v3.0.2 FCST.grb -set_ext_num 1 -netcdf FCST.nc
     lost fields because the field names were not unque

  $ wgrib2.v3.0.2 FCST.grb -set_ext_num 5 -netcdf FCST.nc
     extended name include misc-info and forecast-time-info.
```

The final modification to the extended name, was to make the field and space
character a run-time parameter by the option, -set_ext_name_chars. The default
values are '.' and '\_' for backwards compatibilty. The modification was needed
we now are seeing level-info and misc-info with periods in them like "0.5 mb".

### -match, -if, and other string matches

The extended names also applies the match inventory. So string matches will have
to be rewritten. Fortunately the process is mechanical.

```
  -match ":RH:"         ->     -match ":RH(:|.)"
  -match ":RH:500 mb:"  ->     -match ":RH(:|.)" -match ":500 mb:"
```

Many options use the extended name if enabled by -set_ext_name. For example, the
-netcdf uses the extended name if enabled.

### Examples

```
-sh-2.05b$ ./wgrib2 chem.grb2 -var
1:0:MASSDEN
-sh-2.05b$ ./wgrib2 chem.grb2 -ext\_name
1:0:MASSDEN.hi-res_ctl.Water_Vapour
-sh-2.05b$ ./wgrib2 chem.grb2 -set\_ext\_name 1 -ext\_name
1:0:MASSDEN.hi-res_ctl.Water_Vapour
```

See also:
[-ext_name](./ext_name.md)
[-set_ext_name_chars](./set_ext_name_chars.md).

---

> Description: init X X=type ext_name (1*misc+2*level+4\*ftime)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_ext_name.html>_
