# wgrib2: -append, -no_append

## Introduction

The -append and -no_append options
sets/clears the append flag. The [out](./types.md) options are
expected to respect this flag when opening output files. So an -append option
before writing (-text, -bin, etc) should append to a currently existing file.
The -no_append directs the file to be created before use which is the default.

```
$ wgrib2 grb2 -d 1 -append -text all.txt -no\_append -text rec.txt
```

The above line will append record #1 to all.txt and write record #1 to new file rec.txt

## Usage

```
-append
      append to output files

-no_append
      over-write the output files
```

See also:

---

> Description: init append mode, write to existing output files

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/append.html>_
