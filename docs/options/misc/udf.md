# wgrib2: -udf

## Introduction

The -udf option is only available when the UDF
(User Defined Function) extension is installed.
The -udf option runs a shell command (arg1) and then
sets the grid point so to contends of a binary file (arg2).

## Usage

```

-udf STRING FILE

STRING is a shell command
FILE is a binary file which contains the contents of grid points

```

Some functions can be written in RPN and others are more
conveniently writen is a language like C for Fortran.
The idea behind UDF is that the input file to the user
define function is prepared while scanning the grib file.
When the input file is complete, the user defined function
is called. If the user defined function returns no results, then
the -sys option is used to run the user
defined function otherwise -udf is used.

### Comments

The UDF options may not work in windows.

The use of the UDF options is limited and the UDF options should, IMHO,
not be enabled unless the UDF options are needed.

See alse: [-udf_arg](./udf_arg.md),
[-udf](./udf.md),

---

> Description: misc X Y run UDF, X=program+optional_args, Y=return file

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/udf.html>_
