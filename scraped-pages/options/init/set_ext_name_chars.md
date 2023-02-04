# wgrib2: -set_ext_name_chars

## Introduction

The extended names have a simple format. Options of wgrib2 are executed,
and the fields are combined

```

-set_ext_name 1             -var -misc         field separator = '.', space replacement = '_'

PRMSL.ENS=+19               PRMSL:ENS=+19
PRATE.90%_level             TPRATE:90% level

-set_ext_name 2             -var -lev          field separator = '.', space replacement = '_'

HGT.200_mb                  HGT:200 mb
TPRATE.surface              TPRATE:surface

-set_ext_name 2             -var -ftime        field separator = '.', space replacement = '_'

HGT.180_hour_fcst           HGT:180 hour fcst

```

The default field separator and space replacement can may be used
in the various fields such as "1.5 mb. The -set_ext_name_chars
allows you to alter the field separator and space replacement characters.

## Usage

```

-set_ext_name_chars 'X' 'Y'
    X and Y are characters
    The default field separator '.' is replaced by 'X'.
    The default space separator '_' is replaced by 'Y'.


```

### Example

```

$ wgrib2 gep19.aec -set\_ext\_name 3 -ext\_name -set\_ext\_name\_chars '=' '~' -var -misc -lev
1:0:HGT=ENS=+19=200~mb:HGT:ENS=+19:200 mb
2:70707:TMP=ENS=+19=200~mb:TMP:ENS=+19:200 mb
3:96843:RH=ENS=+19=200~mb:RH:ENS=+19:200 mb

 The -var -misc -lev fields:  HGT:ENS=+19:200 mb
 are converted to an extended name: HGT=ENS=+19=200~mb
 using the field separater '=' and space replacement of '~'

```

See also: [-s](./s.md)
[-set_ext_name](./set_ext_name.md).
[-set_ext_name_chars](./set_ext_name_chars.md).
[-ext_names](./ext_name.md).

---

> Description: init X Y extended name characters X=field Y=space

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_ext_name_chars.html>_
