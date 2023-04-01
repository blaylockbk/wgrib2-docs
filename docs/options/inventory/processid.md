# -processid

## Introduction

The -processid option prints

```
Background generating process identifier (defined by originating centre)
Analysis or forecast generating process identified (see Code ON388 Table A)
```

which are octets 13 and 14 in section 4 in many of the Product Definition Templates
such 4.0.

## Usage

```
-processid
```

### Example

```
$ wgrib2 png.grb2 -pdt
1:4:background generating process=0 forecast generating process=80
```

See also:

---

> Description: inv process id (locally defined)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/processid.html>_
