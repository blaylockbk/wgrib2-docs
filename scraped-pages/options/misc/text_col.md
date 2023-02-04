# wgrib2: -text_col

## Introduction

Wgrib2's -text and -ijbox options
will/can write text files. The default is to write one number per line of output.
By using -text_col option, you can write N numbers per line.

## Usage

```

-text_col N
          N is an integer greater than zero

```

### Example

```

$ wgrib2 cdas.t00z.sfluxgrbf06.grib2 -d 1 -text\_col 3 -text junk
1:0:d=2016122700:UFLX:surface:0-6 hour ave fcst:
$ head junk
192 94
0.037 0.035 0.034
0.032 0.031 0.029
0.028 0.026 0.025
0.023 0.022 0.019
0.016 0.015 0.014
0.013 0.015 0.015
0.013 0.012 0.011
0.009 0.008 0.006
0.005 0.003 0.002

```

See also:
[-text](./text.md),
[-ijbox](./ijbox.md)

---

> Description: misc X number of columns on text output

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/text_col.html>_
