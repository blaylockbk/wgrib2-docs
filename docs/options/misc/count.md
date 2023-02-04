# wgrib2: -count

## Introduction

The -count option just counts the number
of fields. It is very similar to the piping the output of
wgrib to the "wc -l". The advantage of using
-count is that the inventory is displayed
and the count goes to stderr. By retaining stderr, one
could make sure the expected number of records were processed.

## Usage

```
-count
```

### Example

```
$ wgrib2 fcst.grb2 -count
-sh-3.00$ wgrib2 fcst.grb2 -count
1:4:d=2007032600:HGT:1000 mb:anl:
2:422561:d=2007032600:HGT:1000 mb:3 hour fcst:
number of records: 2
```

The above example shows that there were two records in the file.

```
$ wgrib2 fcst.grb2 -count -match fcst
2:422561:d=2007032600:HGT:1000 mb:3 hour fcst:
number of records: 1
```

The above example shows that only 1 record had the string fcst in it.

See also: [-grid_changes](./grid_changes.md)

---

> Description: misc prints count, number times this -count was processed

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/count.html>_
