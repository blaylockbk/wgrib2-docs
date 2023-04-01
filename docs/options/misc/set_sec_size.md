# -set_sec_size

## Introduction

The -set_sec_size option is DANGEROUS.
Grib messages are made of sections where the section number varies from
0 to 8. Suppose you want to modify a grib message by changing
section 4 (Product Definition Section). You would
use -set_sec_size to change the size of section 4
if necessary and then use -set_byte to change the contents
of your new section 4. This is not pretty but when testing a new template,
you have to do ugly things.

After you have altered the grib message, you can save the
message by either -grib or
the -grib_out. You need to use
the latter option if the grid values were altered because the
data section needs to be updated.

## Usage

```
-set_sec_size  SECTION SIZE
SECTION=0 .. 8
SIZE=integer, size of new section
     can be zero for a missing section
     generally greater than 5
```

### Results

The -set_sec_size option expands
or contracts a section. For expansions, the new octets
are filled with 255. This option puts the size of the
new section in octets 1..4 and the section number in octet 5
assuming the size is greater than equal to 5. Some
sections can be missing.

See also:
[-set_byte](set_byte.md)

---

> Description: misc X Y resizes section , X=section number, Y=size in octets, DANGEROUS

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_sec_size.html>_
