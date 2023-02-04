# wgrib2: -new_grid_vectors

## Introduction

The -new_grid_winds option selects the wind rotation/orientation
for the -new_grid option. This orientation should apply to all
vector quantities. However, the grib format doesn't have a flag specifying which fields
are vector fields. Sometimes you like to treat vector quanties as scalars because it
makes interpolation easier (for example a time series of tropical U fields). Another
complication is that one often wants to duplicate the default action of copygb
which is to only treat U and V as vectors.

The -new_grid_vectors option allows you to select which fields
will be interpolated as vectors. The options are,

1. none              -all fields are interpolated as scalars

- UGRD:VGRD  -only UGRD and VGRD are interpolated as vectors (like default copygbfault behavior)
- default (2.0.1 and prior)     UGRD,VGRD,UVCSH,VVCSH,UFLX,VFLX,UGUST,
  VGUST,USTM,VSTM,VDFUA,VDFVA,UOGRD,VOGRD are interpolated as vectors
- default (2.0.2)         UGRD,VGRD,UVCSH,VVCSH,UFLX,VFLX,UGUST,
  VGUST,USTM,VSTM,VDFUA,VDFVA,UOGRD,VOGRD,MAXUW,MAXVW are interpolated as vectors
- default (2.0.3-2.0.6)         UGRD,VGRD,UVCSH,VVCSH,UFLX,VFLX,UGUST,
  VGUST,USTM,VSTM,VDFUA,VDFVA,UOGRD,VOGRD,MAXUW,MAXVW,UICE,VICE,U-GWD,V-GWD
  are interpolated as vectors
- default (2.0.7+)         UGRD,VGRD,UVCSH,VVCSH,UFLX,VFLX,UGUST,
  VGUST,USTM,VSTM,VDFUA,VDFVA,UOGRD,VOGRD,MAXUW,MAXVW,UICE,VICE,U-GWD,V-GWD,USSD,VSSD
  are interpolated as vectors

## Usage

```

-new_grid_vectors X
    X = none, default, UGRD:VGRD, UV list
    example UV list:   "UGRD:VGRD:UICE:VICE"

```

See also: [-new_grid](./new_grid.md),
[-new_grid_interpolation](./new_grid_interpolation.md)

---

> Description: misc X change fields to vector interpolate: X=none,default,UGRD:VGRD,(U:V list)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/new_grid_vectors.html>_
