# wgrib2: -set_var

## Introduction

Please see [new grib](./new_grib.md) for the basic
concepts of making new grib files.

The -set_var option changes the variable
name of the in-memory grib (sub-)message. You can write
out the message with the new name by the
-grib (fast) and
-grib_out (slow) options. Of course
if the in-memory grid values have changed, you have to use
the latter option.

You can set the name to a locally defined variable name only if
the center is set correctly. For example, you can use an NCEP-defined
variable name only if the center is already defined as NCEP. If the
center is undefined, you cannot use any locally defined names.

The search order is for WMO definitions and then the locally
defined definitions. The search order has implications for NCEP
users. NCEP often have WMO and NCEP definitions for the same
variable name/type.
The -set_var option will choose
the WMO definition. This will cause problems with programs that
expect the NCEP definition.

The -set_var option will alter the master table.
The grib2 standard says that variable definitions are only valid for
specified master tables. Wgrib2 keeps track of range of tables for which each
variable name is valid. In addition, there is an entry for the master table
used by the -set_var option.

## Usage

```

-set_var X              X=valid grib variable name such
                        1. NCEP-defined text name such as HGT or TMP
                            local variable names have to consistent with the center
                        2. varA_B_C_D_E_F
                        3. var discipline=A master_table=B parmcat=D parm=E
                        4. var siscipline=A local_table=C parmcat=D parm=E
                           use 3 for WMO defined variables, 4 for locally defined variables
                           A = discipline
                           B = version of master table
                           C = version of local table
                           D = center
                           E = parameter
                           F = category
                        formats 2-4 were introduced with wgrib2 v2.0.7

```

### Example

```

$ wgrib2 p.grb
1:0:d=2009072100:PRES:mean sea level:anl:
$ wgrib2 p.grb -set\_var TMP -grib out.grb
1:0:d=2009072100:TMP:mean sea level:anl:
$ wgrib2 out.grb
1:0:d=2009072100:TMP:mean sea level:anl:

You can use -set_var to change from old variable names to the new variable names
by the appropriate use of the -if option.
Convert TSOIL (old) to SOILTMP (new)

$ wgrib2 old.grb -if ":TSOIL:" -set\_var SOILTMP -fi -grib new.grb


```

The -set_var option will rename
all the fields in a grib file. If you only want to rename
specific fields, you will have to use the
-if and -fi options.

See also:

[-fi](fi.md),
[-grib](grib.md),
[-grib_out](grib_out.md),
[-if](if.md)
[-set_metadata](set_metadata.md)

---

> Description: misc X changes variable name

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_var.html>_
