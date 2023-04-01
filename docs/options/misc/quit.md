# -quit

## Introduction

The -quit option forces a quit and the
end of the processing of the current line. You would use
the -quit option to save time (see
2nd example).

## Usage

```
-quit
```

### Examples

```
$ wgrib2 /tmp/pgb.f0009031206.g2 -quit
1:4:d=2009031206:HGT:1000 mb:anl:
```

This has the same effect as -d 1.

```
$ wgrib2 /tmp/pgb.f0009031206.g2 -match ":RH:1000 mb:" -quit
64:466700:d=2009031206:RH:1000 mb:anl:
```

This forces a quit after the first "RH:1000 mb" is processed. This can save time
when there is only one match.

```
$ wgrib2 /tmp/pgb.f0009031206.g2 -if "^3:" -quit -if
1:4:d=2009031206:HGT:1000 mb:anl:
2:5399:d=2009031206:HGT:925 mb:anl:
3:10589:d=2009031206:HGT:850 mb:anl:
```

This forces the quit after the third line. This has the same effect as
-for 1:3.

See also: [-if](./if.md),
[-for](./for.md)
[-alarm](./alarm.md)
[-limit](./limit.md)

---

> Description: misc stop after first (sub)message (save time)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/quit.html>_
