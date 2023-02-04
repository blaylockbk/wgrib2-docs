# wgrib2: -import_bin, -import_ieee, -import_text, -import_grib

## Introduction

Wgrib2 will decode the grib message and save the decoded
grid point values in a floating point array (DATA). The -import
options read grid point values from a specified file and replace the values of DATA.
The size of DATA and imported grid should match. The -import
options are often used to read data that is later written out as a grib message.

Note that the import functions will reset the scaling and precision of the grib writing (new files)
to the default (ECMWF-style, 12 bits). Any -set_metadata should be done after the -import functions.

### Scan Order

The grib message's scan order is called the "input" scan order (wgrib2 -grid).
Wgrib2 converts this to the "output" scan order. (This is the scan order for
options like: -bin, -text, -cvs.) The import file needs to be in the "output"
scan order. Of course, you can change the output scan order using the -order
option.

### import Format

The file that you import needs to be in a special format.

- grib: grib2 message
- bin: native single point format
- ieee: IEEE single point format
- bin: may have a f77 style header depending on the -header option
- ieee: may have a f77 style header depending on the -header option
- ieee: may be little or big (default) ending depending on options
- text: may have a "nx ny" header depending on the -header option
  (see -text option)

## Usage

```

-import_bin data.bin              .. a binary (native format floating point) file
-import_ieee data.ieee            .. a big-endian 32-bit ieee file
                                     note that the -header and -no_header affect the data file format
-import_text data.txt		  .. a text file
                                     note that the -header and -no_header affect the data file format
-import_grib data.grb		  .. a grib2 file
                                     -g2clib 2 is not supported
                                     conversion to we:sn and we:ns is supported
                                     reads next grid (message or submessage)
                                     -not and -match do not affect -import_grib

Note: grid size (if it can be determined) must match the current grid.

```

### Example 1

```

wgrib2 IN.grb -bin dump.bin              .. make a binary sequential file of the fields
Fix.sh dump.bin dump.bin.new             .. alter dump.bin
wgrib2 IN.grb -set_grib_type same -import_bin dump.bin.new -set_scaling same same -grib_out IN.new.grb

```

The -import options reads the data for one field and overwrites
the current grid point values. The -grib_out option writes a new
grib message with the new grid point values.

### Example 2: using import to write a grib file

```

template.grb2             is a single message (field/record) grib2 file with the appropriate grid
                          usually a simple type like 6 hour forecast or analysis
new.bin                   is a binary file with a single field with th same grid as template.grb2


wgrib2 template.grb2 -import_bin new.bin -set_var GEOLAT -set_lev "surface" -set_ftime "anl" \
     -set_date 1999123112 -set_grib_max_bits 24 -set_bin_prec 24 -grib_out new.grb2

This commands writes new.bin as a grib2 file as GEOLAT at 12Z Dec 31, 1999 using 24 bits precision.

```

See also:
[-import_netcdf](./import_netcdf.md),
[-set_grib_type](./set_grib_type.md),
[-set_scaling](./set_scaling.md),
[-undefine](./undefine.md),
[-grib_out](./grib_out.md),
[-set_date](./set_date.md),
[-set_ftime](./set_ftime.md),
[-set_lev](./set_lev.md),
[-set_var](./set_var.md),

---

> Description: misc X read ieee file (X) for data

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/import_ieee.html>_
