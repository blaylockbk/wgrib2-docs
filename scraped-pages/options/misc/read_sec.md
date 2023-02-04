# wgrib2: -read_sec

## Introduction

The -read_sec option will replace the section (0-8) of the current grib message
with the contents from a file. The only options that applies to the file are the -header
and -no_header options.
The default -header option put a 4-byte unsigned integer header and trailer around the section data.
The header and trailer are the number of bytes of the section.

The -read_sec option is used to create grib files where you do not an appropriate template.

## Usage

```

-read_sec N file            N = 0..8
                            file = file to read

```

### Example 1

```

wgrib2 IN.grb -read_sec 0 sec0.dat -read_sec 1 sec1.dat -read_sec 2 sec2.dat -read_sec 3 sec3.dat \
  -read_sec 4 sec4.dat -read_sec 5 sec5.dat -read_sec 6 sec6.dat -read_sec 7 sec7.dat -grib OUT.grb

```

See also:
[-write_sec](./write_sec.md),

---

> Description: misc X Y read grib message section (0-8) X from binary file (Y)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/read_sec.html>_
