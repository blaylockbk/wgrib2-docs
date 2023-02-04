# wgrib2: -import_grib_fs (wgrib2 v3.0.0+)

## Introduction

The -import_grib_fs option sets the file pointer
to the beginning of the file, and reads sequentially until it finds
a record that matches the search string. It like a
-if_fs for reading a grib file.

The -import_grib_fs option is more useful
verion of -import_grib.

## Usage

```

-import_grib_fs STRING data.grb	     reads a grib message that matchs STRING from file data.grb
				     the match is by fixed string, not regular expression
                                     -g2clib 2 is not supported
                                     conversion to we:sn and we:ns is supported
                                     starts at the beginning of the file
				     reads sequentially
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


wgrib2 template.grb2 -import_bin new.bin -set_var TMP -set_lev "2 m above ground" -set_ftime "anl" \
     -set_date 1999123112 -grib_out new.grb2

This commands writes new.bin as a grib2 file as TMP2m analysis at 12Z Dec 31, 1999.

```

See also:
[-import_grib](./import_grib.md),

---

> Description: misc X Y read grib2 file (Y) sequentially for record that matches X (fixed string)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/import_grib_fs.html>_
