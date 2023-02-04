# wgrib2: -write_sec

## Introduction

The -write_sec option writes the current grib message section (0-8)
to file. The only options that applies to the file are the -header,
-no_header, -append, and
-no_append options.
The default -header option put a 4-byte unsigned integer header and trailer around the section data.
The header and trailer are the number of bytes of the section.

The -write_sec option is by codes that need low-level access to the grib data.

## Usage

```
-write_sec N file            N = 0..8
                             file = file to write
```

### Example 1

```
wgrib2 IN.grb -write_sec 0 sec0.dat -write_sec 1 sec1.dat -write_sec 2 sec2.dat -write_sec 3 sec3.dat \
  -write_sec 4 sec4.dat -write_sec 5 sec5.dat -write_sec 6 sec6.dat -write_sec 7 sec7.dat
```

See also:
[-read_sec](./read_sec.md),

---

> Description: out X Y write grib msessage section X (0-8) to binary file Y

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/write_sec.html>_
