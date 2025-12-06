# Best practices

## File Suffix

GRIB2 files should have the suffix `.grib2`

Index files should have the suffix `.grib2.idx`, using the same name as the GRIB2 it was derived from.

## Standard Index Files

Standard index files are made with the [`-s`](../options/inventory/s.md), simple inventory option:

```bash
wgrib2 myFile.grib2 -s > myFile.grib2.idx
```
