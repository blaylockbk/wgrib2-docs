# wgrib2: -gribtable_used

## Introduction

The grib variable table that is built in to wgrib2 is based on the NCEP table
and entries from other centers. For the WMO-defined entries, NCEP
names are used. You may want to change the grib table because

1. grib table is incomplete

- you want to use names that make sense
- the center's names for locally defined fields are not included in wgrib2
- you want to use an unofficial table

To solve this problem, you can set up user grib tables.

<https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/user_grib2tables.html>

Now the -gribtable_used option helps you set up the user
grib table. The option writes grib table entries used by a file.

## Usage

```

-gribtable_used OUTPUT
     OUTPUT is a file with the grib table entries for each field

```

### Example

```

$ wgrib2 gep19.t00z.pgrb2af180 -gribtable\_used junk
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:46042:d=2009060500:TMP:200 mb:180 hour fcst:ENS=+19
3:63079:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
..

$ sort -u junk > grbtbl     (get rid of duplicate entries)
edit grbtbl, change UGRD to U, VGRD to V and TMP to T

$ export grib2table=`pwd`/grbtbl      (define a user grib table)

$ wgrib2 gep19.t00z.pgrb2af180 -match '(U|V|T)'
1:0:d=2009060500:HGT:200 mb:180 hour fcst:ENS=+19
2:46042:d=2009060500:T:200 mb:180 hour fcst:ENS=+19
3:63079:d=2009060500:RH:200 mb:180 hour fcst:ENS=+19
4.1:86046:d=2009060500:U:200 mb:180 hour fcst:ENS=+19
4.2:86046:d=2009060500:V:200 mb:180 hour fcst:ENS=+19
5:137483:d=2009060500:HGT:250 mb:180 hour fcst:ENS=+19
6:184669:d=2009060500:T:250 mb:180 hour fcst:ENS=+19
..

Note: U and V will not be treated as vectors in -new_grid.  You have
to use -new_grid_vectors to get U and V to be vectors.

```

See also: [-new_grid](./new_grid.md),
[-new_grid_vectors](./new_grid_vectors.md),
[user_grib2tables](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/user_grib2tables.md)

---

> Description: out X write out sample gribtable as derived from grib file, X=file

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/gribtable_used.html>_
