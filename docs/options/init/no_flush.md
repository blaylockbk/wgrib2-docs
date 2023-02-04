# wgrib2: -no_flush

## Introduction

The -no_flush option causes wgrib2 to flush
the output buffers when the buffers are full or the program ends.
This is the opposite of the The -flush option.

The only practical use of this option would be in a non-POSIX
system where the flush mode is turned on, you are not using output
pipes, and you wanted to speed up the output. For this option
to be effective, the option has to be the last option that
uses a file. (Opening a file can update the flush mode.)

## Usage

```
-no_flush
  should the last option
```

---

> Description: init flush output buffers when full (default)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/no_flush.html>_
