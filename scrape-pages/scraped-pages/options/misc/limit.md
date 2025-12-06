# wgrib2: -limit

## Introduction

The -limit option stops the processing of the grib file after N messages/submessages have been decoded. The purpose of this function
to limit amount of CPU time that a web server may devote to an individual
request.

The newer -alarm option is better suited for stopping jobs web jobs.

## Usage

```
-limit N    where N is an integer.
```

See also: [-quit](./quit.md),
[-alarm](./alarm.md)

---

> Description: misc X stops after X fields decoded

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/limit.html>_
