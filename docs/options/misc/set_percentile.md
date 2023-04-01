# -set_percentile

## Introduction

Probabilistic forecasts can be presented as percentiles. For example, 10% forecast
of temperature means that 1 time out of 10, the expected temperature will less than
the forecasted temperature.
The -set_percentile option adds or changes the percentile of
a forecast.

Percentile forecasts use product definition templates (PDT) of 6 or 10 depending
whether the forecast is for a single time or a time interval.
The -set_percentile option converts pdt 0..6 -> 6 and 8..15 -> 10.

Percentiles are also supported by the
-set_metadata option.

## Usage

```
-set_percentile X         X = percentile 0..100
```

### Example

```
$ wgrib p.grb -set\_percentile 50 -grib\_out new.grb
1:0:d=2010020304:PRMSL:mean sea level:6 hour fcst:50% level
2:17960:d=2010020304:PRES:1 hybrid level:3 hour fcst:50% level
```

See also:

[-grib](grib.md),
[-grib_out](grib_out.md),
[-set_metadata](set_metadata.md),

---

> Description: misc X convert PDT 0..6 -> 6, 8..15 -> 10, X=percentile (0..100)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_percentile.html>_
