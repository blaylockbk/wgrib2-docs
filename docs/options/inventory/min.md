# -stats, -max, -min

## Introduction

The -stats option writes a statistical summary
of the field into the inventory.
The -max option writes the maximum value and
the -min option writes the minimum.
These option are useful for quickly determining if the field has reasonable numbers.

## Usage

```
-stats
-max
-min
```

### Example

```
$ wgrib2 test.grb2 -stats
1:0:ndata=65160:undef=0:mean=83.8696:min=-428.1:max=317.8

ndata = number of grid points
undef = number of grid points with an undefined value
mean = grid point average (not area weighted)
min = minimum value
max = maximum value

$ wgrib2 test.grb2 -max
1:0:max=317.8

$ wgrib2 test.grb2 -min
1:0:min=-428.1

```

The -stats option can be combined with
the -undefine option to produce
statistics for a box.

```
$ wgrib2 test.grb2 -stats
1:4:ndata=10512:undef=0:mean=77.5081:min=-370:max=340.2:cos_wt_mean=97.267
```

produces the global statistics. By setting grid points to undefined,
we can produce the statistics for a box.

```
$ wgrib2 test.grb2 -undefine outobx 0:90 -10:10 -stats
1:4:ndata=10512:undef=10179:mean=79.8829:min=41:max=144:cos_wt_mean=79.8688
```

Note, if we reverse the order of the
-stats and
-undefine options, we get the global mean.
That is because the -stats option is excuted
before the -undefine options.

$ wgrib2 test.grb2 -stats -undefine out-box 0:10 -10:10
1:4:ndata=10512:undef=0:mean=77.5081:min=-370:max=340.2:cos_wt_mean=97.267

If all the data are undefined,
the -stats option will produce values
of zero for the the mean, min and max.
The -min and -max
options will yield a text string of "undefined".

See also: [-undefine](./undefine.md)

---

> Description: inv print minimum value

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/min.html>_
