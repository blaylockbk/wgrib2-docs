# wgrib2: -subcenter

## Introduction

The -subcenter option prints out the subcenter.
For example, within NCEP, the subcenters include NCO, EMC, HPC, MPC, CPC, etc.

## Usage

```

-subcenter

```

### Example

```

$ wgrib2 test.grb2 -subcenter
1:0:subcenter=NCEP Ensemble Products
2:46042:subcenter=NCEP Ensemble Products
3:63079:subcenter=NCEP Ensemble Products
4.1:86046:subcenter=NCEP Ensemble Products
...

```

See also: [-center](./center.html),
[-set](./set.html)

---

> Description: inv subcenter

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/subcenter.html>_
