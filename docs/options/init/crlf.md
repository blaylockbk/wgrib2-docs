# wgrib2: -crlf

## Introduction

On unix/linux machines, wgrib2 terminates the end of a inventory line by a LF (newline or "\n").
On Windows machines, wgrib2 terminates the end of a inventory line by a CRLF.
The -crlf option is for unix/linux machine so that they terminate
the line by CRLF. This will make the inventory Windows compatible.

## Usage

```
-crlf
```

See also:

---

> Description: init make the end of the inventory a crlf (windows) instead of newline (unix)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/crlf.html>_
