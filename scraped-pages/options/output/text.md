# wgrib2: -text

## Introduction

The -text option writes the grid values to a specified
file as a text (ascii) file. The default format (-header) is (per decoded grib message)

```

   NX NY
   val-1
   val-2
   ...
   val-(NX*NY)

```

Where NX and NY are the dimension of the grid and val-N is the Nth grid
point in WE:SN order. When the -no_header option is used, the format is

```

   val-1
   val-2
   ...
   val-(NX*NY)

```

The default order of the data is WE:SN which differs from
wgrib's order which is equivalent to the "-order raw" in wgrib2.
Another difference is that wgrib's -text option requires a filename.
The undefined value is 9.999e20. The format of the output is unchanged
from wgrib except the order, number of columns and precision can
be altered by the user.

## Usage

```

-text file_name
      "-" sends the data to the terminal/stdout

```

### Example

```

$ wgrib2 test.grb2 -s | grep ":RH:2 m" | wgrib2 -i test.grb2 -text data.txt
285:36796469:d=2005090200:RH:2 m above ground:60 hour fcst

```

The above line extracts the 2 meter RH from file test.grb2 and writes it in data.txt

See also:
[-netcdf](./netcdf.html),
[-spread](./spread.html),
[-bin](./bin.html),
[-ieee](./ieee.html)

---

> Description: out X write text data into X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/text.html>_
