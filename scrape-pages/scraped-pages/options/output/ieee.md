# wgrib2: -ieee

## Introduction

The -ieee option writes the grid values to a specified
file in IEEE format (C: float, Fortran: real). The default endian is "big",
but that can be changed by the
-big_endian and
-little_endian options.

By default, the data are written out in
WE:SN order (see -order option) with f77 header/trailer (see -no_header option). The
undefined value is 9.999e20. The output format is unchanged from wgrib except
the order can now be specified.

The default is to write a 4 byte header and trailer with the record size in bytes.
No header and trailer are written if the header flag is off by
the -no_header option.

The default is write the grid point data in WE:SN order. The ordering of the data
can be changed to raw or WE:NS by the -order option.

The default endian of the header, trailer and data are in big endian order. This can
be changed to little endian by the -little_endian option.

The -ieee option is slower than the -bin option and may not be exact on an ieee
machine because all grid point data is converted from the native format into ieee format
by a software routine.

## Usage

```
-ieee file_name
```

### Example

```
$ wgrib2 test.grb2 -s | grep ":RH:2 m" | wgrib2 -i test.grb2 -ieee data.bin
285:36796469:d=2005090200:RH:2 m above ground:60 hour fcst
```

The above line extracts the 2 meter RH from file test.grb2 and writes it in data.bin

```

      wgrib                ==>                 wgrib2

      -header -ieee -o out.bin                 -header -order raw -ieee out.bin
      -ieee -o out.bin                         -order raw -ieee out.bin

```

See also: [-text](./text.md),
[-netcdf](./netcdf.md)
[-spread](./spread.md)
[-bin](./bin.md)
[-big_endian](./big_endian.md)
[-little_endian](./big_endian.md)

---

> Description: out X write (default:big-endian) IEEE data to X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ieee.html>_
