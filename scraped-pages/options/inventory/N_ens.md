# wgrib2: -N_ens

## Introduction

The -N_ens option prints the number of members
in the ensemble (ensemble forecasts only).

## Usage

```

-N_ens

```

### Example

```

$ wgrib2 in.grb -N\_ens
1:80:10 ens members
2:218826:10 ens members
3:434430:10 ens members
..

for wgrib2 upto 2.0.7, will print -1 ens members for product definition tables that
do not support number of ensemble members. For wgrib2 2.0.8 and following, the field
will be empty.

```

See also:
[-ens.html](ens.html)
[-set_ens_num.html](set_ens_num.html)

---

> Description: inv number of ensemble members

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/N_ens.html>_
