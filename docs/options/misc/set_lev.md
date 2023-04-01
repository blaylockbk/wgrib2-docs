# -set_lev

## Introduction

The -set_lev option changes the level/layer
of field. By design, the level/layer used by
-set_lev option is the same level/layer as used
in the inventory.

The code used to set the level/layer are used by
-set_lev,
-set_metadata, and
-set_metadata_str. So if you are unsure of the format
of the level description, you first look for a file with the same or similar level.
Failing to find such a file, you can try the
-set_lev option or look at the tables in level.c source code.

## Usage

```
-set_lev LEVEL
  LEVEL is either a level or layer description (12/2015)

 "surface",
 "cloud base",
 "cloud top",
 "0C isotherm",
 "level of adiabatic condensation from sfc",
 "max wind",
 "tropopause",
 "top of atmosphere",
 "sea bottom",
 "entire atmosphere",
 "cumulonimbus base",
 "cumulonimbus top",
 "%g K level",
 "%g mb",
 "mean sea level",
 "%g m above mean sea level",
 "%g m above ground",
 "%g sigma level",
 "%g hybrid level",
 "%g m underground",
 "%g K isentropic level",
 "%g mb above ground",
 "PV=%g (Km^2/kg/s) surface",
 "%g Eta level",
 "logarithmic hybrid level",
 "snow level",
 "mixed layer depth",
 "hybrid height level",
 "hybrid pressure level",
 "%g generalized vertical height coordinate",
 "%g m below sea level",
 "%g m below water surface",
 "lake or river bottom",
 "bottom of sediment layer",
 "bottom of thermally active sediment layer",
 "bottom of sediment layer penetrated by thermal wave",
 "maxing layer",
 "bottom of root zone",
 "top surface of ice on sea, lake or river",
 "top surface of ice, und snow on sea, lake or river",
 "bottom surface ice on sea, lake or river",
 "deep soil",
 "top surface of glacier ice and inland ice",
 "deep inland or glacier ice",
 "grid tile land fraction as a model surface",
 "grid tile water fraction as a model surface",
 "grid tile ice fraction on sea, lake or river as a model surface",
 "grid tile glacier ice and inland ice fraction as a model surface",
   ** NCEP specific special levels are not listed **
 "%g-%g mb above ground",
 "%g-%g mb"
 "%g-%g m below ground"
 "%g-%g m above ground"
 "%g-%g sigma layer",
 "%g-%g m below sea level",
 "%gC ocean isotherm%n", NCEP only
 "atmos col"
```

### Example

```
$ wgrib2 png.grb22 -set\_lev "9.1 mb"
1:4:d=2009060500:RH:9.1 mb:330 hour fcst:ens std dev
```

See also:
[-lev](lev.md),

---

> Description: misc X changes level code .. not complete

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_lev.html>_
