# Migrate from wgrib to wgrib2

## Changed options

Converting scripts that use wgrib to wgrib2 should be straight forward.

| wgrib                       | wgrib2                                 |
| --------------------------- | -------------------------------------- |
| -d all                      | (no option needed)                     |
| -d N                        | -d N or -d N.M (for grib2 submessages) |
| -bin -o FILE.BIN            | -bin FILE.BIN                          |
| -text -o FILE.TXT           | -text FILE.TXT                         |
| -ieee -o FILE.BIN           | -ieee FILE.BIN                         |
| -grib -o FILE.GRIB          | -grib FILE.GRIB                        |
| -nh                         | -no_header                             |
| -h                          | -header                                |
| -verf (sets verf time flag) | -verf (write inventory with verf time) |
| -s -verf                    | -verf                                  |
| -PDS/-PDS10                 | n/a, use -get_bytes                    |
| -GDS/-GDS10                 | n/a, use -get_bytes                    |
| -ncep_opn/-ncep_rean        | not needed                             |
| -4yr                        | not needed, -4yr is always set         |
| -ncep_ens                   | n/a, not needed                        |
| -p                          | n/a                                    |
| -dwdgrib                    | n/a, not needed                        |
| -H                          | n/a                                    |
| -o                          | not needed, syntax change              |

Note: In wgrib2, grids are converted to `we:sn` order by default. Use `-order we:ns` for GFS, nothing for NAM.

## Changed inventory format, different searches

The wgrib2 inventory has changed. The various grep/egreps will have to be
changed to see if they are compatible with new inventory format

works

```bash
wgrib  FILE | grep ":HGT:" | wgrib -i  FILE -bin -o FILE.BIN
wgrib2 FILE | grep ":HGT:" | wgrib2 -i FILE -bin FILE.BIN
wgrib2 FILE -bin FILE.BIN -match ":HGT:"
```

works:

```bash
wgrib -4yr FILE | grep ":d=2006081712:" | wgrib -i  FILE -bin -o FILE.BIN
wgrib2 FILE | grep ":d=2006081712:" | wgrib2 -i  FILE -bin -o FILE.BIN
wgrib2 -match ":d=2006081712:" FILE -bin -o FILE.BIN
```

wgrib2 uses a 4 digit year code. Scripts using 2 digit years need to modified.

convert:

```bash
wgrib  FILE | grep ":d=06081712:" | wgrib -i  FILE -bin -o FILE.BIN
wgrib2 FILE | grep ":d=2006081712:" | wgrib2 -i  FILE -bin -o FILE.BIN
wgrib2 -match ":d=2006081712:" FILE -bin FILE.BIN
```

wgrib2 doesn't print out kdps5 .. kpds7 which are not applicable to grib2.

convert:

```
wgrib  FILE | grep "kpds5=7:kpds6=100:kpds7=500:" | wgrib -i  FILE -bin -o FILE.BIN
wgrib2 -match ":HGT:" -match ":500 mb:" -bin FILE.BIN FILE
```

Note: kpds5/6/7 are table dependent so using HGT/500 mb was more reliable.

## grep/egrep

When you use wgrib, you end up using lots of greps. Many of the greps will have to be rewritten because the text has been altered (ex. "10 m above gnd" -> "10 m above ground"), are gone ("kpds5=6"), or replaced by a new format. GRIB2 is big compared with GRIB1 so things had to change. Generally the wgrib2 version use fewer abbreviations because it is easier to understand and line length is less of an issue with a flexiable inventory format.

## grep versus egrep

Sometimes you want grep (pattern matching) rather than egrep (regular expressions, REGEX). This can happen when your search string could have metacharacters. You can alter the type of search used by the various options by the "-set_regex N" option. With the default value (N=0), extended REGEXs are used (egrep). N=1 gives a pattern match like grep and N=2 gives and extended regex with a need to quote the metacharacters.

## Scan, Order of the Data

In wgrib, files were decoded in the "raw" order; i.e., the order that they were written. For most files the order was we:ns or we:sn. With GRIB2, the complexity of the order was increased. In order to make life easier for the user, wgrib2, by default, put the data in a we:sn order. (There is an option to put the data in a we:ns order.)

## Command line: order of options

With wgrib, option processing was simple. You didn't care where the option went and if you had conflicting options, the last one was used. With wgrib, the flags changed the configuration before the processing of the grib data.

Wgrib2 is a much more dynamic program. Each option now runs a subroutine. As with subroutines, the order is important and the subroutine can be called multiple times. These subroutines are run in following order.

1. **Initialize:** In command-line order with the mode set to Initialize. This
   stage is used to open files, initialize arrays and can be used
   to parse the arguments.

2. **Processing:** In command-line order with mode set to Process and a copy of
   the data and latitudes and longitudes (if requested). This is
   repeated for each field.

3. **Finalize:** Each called routine is executed with the mode set to Finalize.
   This allows the routines to complete any pending operations
   (example, averaging) and free up arrays.

---

> Description: wgrib -> wgrib2

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/convert_wgrib2.html>_
