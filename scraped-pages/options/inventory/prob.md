# wgrib2: -prob

## Introduction

The TMP (temperature) field usually contains the temperature
in Kelvin. However, with the right options, the TMP field
could contain the probability of the temperature being

- below a specified value
- above or a specified value
- between a two limits (lower limit <= V < upper limit)

The -prob option prints out the probability
field, if any.

## Usage

```
-prob
```

### Example

```
sh-2.05b$ wgrib2 sref.t03z.pgrb243.prob.grib2 -prob
1:0:prob <273
2:1378:prob <273
3:2756:prob >500
4:6309:prob >1000
5:8800:prob >2000
6:9801:prob >3000
7:10262:prob >4000
```

Normally -prob in invoked by other
inventory functions like -misc and
-s.

See also:
[-set_prob](./set_prob.md)

---

> Description: inv probability information

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/prob.html>_
