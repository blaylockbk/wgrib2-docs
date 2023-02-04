# wgrib2: -grid_changes

## Introduction

The -grid_changes option is a safety option.
Normally we don't expect the grid to change within a grib file, and many
programs would fail if the grid did change. Wgrib2 will work if the
grid changes but it would work much slower. (Each grid change would
require a recalculation of the grid parameters such as the the
grid point locations if needed.)
The -grid_changes option prints
to stderr, the number of times the grid changed during processing
of the file. Only grib (sub-)messages that were processed and not
skipped by a -match or similar option will count.

## Usage

```

-grid_changes

```

### Example

```

$ wgrib2 fcst.grb2 -bin fcst.bin -grid\_changes -count
-sh-3.00$ wgrib2 fcst.grb2 -bin fcst.bin -grid_changes
1:4:d=2007032600:HGT:1000 mb:anl:
2:422561:d=2007032600:HGT:1000 mb:3 hour fcst:
Good: only one grid
number of records: 2

```

In the above example, we converted the file to binary. It consisted
of a single grid and two records.

See also:
[count](./count.md)

---

> Description: misc prints number of grid changes

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/grid_changes.html>_
