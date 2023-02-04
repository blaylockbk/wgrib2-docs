# wgrib2: user grib2tables

## Introduction

Grib2 messages have variable names such as HGT or TMP. The table built into wgrib2
can be agumented using "user defined grib tables". This allows you to add locally
defined variables as well, update the grib table, and change the names of pre-existing
variables. Wgrib2 looks for the user defined gribtable in (in order of search)

```

   environment varible: GRIB2TABLE
   environment varible: grib2table
   file: grib2table (in current working directory)

   Assuming your user definitions are in a file called "/home/my_name/my_grib2table"

   for sh/bash
     export GRIB2TABLE=/home/my_name/my_grib2table
   and for people who have a broken caps lock key
     export grib2table=/home/my_name/my_grib2table

   for csh
     setenv GRIB2TABLE /home/my_name/my_grib2table
   or
     setenv grib2table /home/my_name/my_grib2table

   You can also always put the user defined gribtable in the "grib2table" in
   the "current working directory".

```

The user defined gribtable is searched before the built-in gribtable.

### Format of the user defined gribtable

```

   Comments
       start with the first column staring with #, *, !
       any line that is not a definition line

   definitions:
       I1:I2:I3:I4:I5:I6:I7:I8:NAME:DESC:UNITS
       I1=discipline
       I2=master table set: used when using -set_var
       I3=master table low:
       I4=master table high:
            According to WMO, the WMO grib definitions depend on the master table
          (Table 1.0).  For example, a (discipline, parameter category, parameter)
          triple may mean different things depending on the master table. Common
          sense suggests that you minimize the number of redefinitions.
            To conform to WMO, the grib variable definitions are valid for a
          range of master table values (I3 and I4).  When setting the variable
          name, the master table is set to I2 for that field.

          defintion valid for low <= mastertable <= high
          want I3 ≤ I2 ≤ I4
          must have I2, I3 and I4 in the range of [0..255]
       I5=center  (use -1 for a wild card)
       I6=local table number
       I7=parameter category (code table 4.1)
       I8=parameter (code table 4.2)

       when a field is not used, use a value of -1
       for locally defined fields, I5 (center) must not be -1

   all other lines are not used.  Warnings will be given when the number of colons suggest an error.

```

For each variable, there is a master table set, low and high. The master table is a master
version number. Each definition has a range of version numbers for which it is recognized.
If you want the variable to defined for all version numbers, set "low" to zero and "high"
to some large positive integer. The "set" value is master table number used when the "-set_var"
option is used change the variable name.

### Locally Defined Variables

The grib2 standard usually allows locally defined variable definitions.
They are defined when the Discipline, or Parameter Category or Parameter Number
ranges from 192-254. When the variable is locally defined, then the
center has to be set (not -1) in the user grib table.. The variable is only
defined for the center. Note: WMO does not not allow the local table to have
a value of zero.

```

Locally defined variables: a or b or c
  a) Discipline: 192-254
  b) Parameter Category: 192-254
  c) Parameter Number: 192-254

```

### Sample grib2table

```

/*
 * sample user grib table
 */
/*

  struct gribtable_s {
    int disc;   /* Section 0 Discipline                                */
    int mtab_set;    /* Section 1 Master Tables Version Number used by set_var      */
    int mtab_low;    /* Section 1 Master Tables Version Number low range of tables  */
    int mtab_high;   /* Section 1 Master Tables Version Number high range of tables */
    int cntr;   /* Section 1 originating centre, used for local tables */
    int ltab;   /* Section 1 Local Tables Version Number               */
    int pcat;   /* Section 4 Template 4.0 Parameter category           */
    int pnum;   /* Section 4 Template 4.0 Parameter number             */
    const char *name;
    const char *desc;
    const char *unit;
  };

*/
0:1:0:10:8:0:190:190:TEST1:Critcal Fire Weather:??
0:1:0:10:8:0:190:191:TEST2:Dry lightning:??
0:1:0:10:0:0:2:2:Utest:utest:m/s

```

### Uses

1. update tables with new or local variables

- convert the variable names to another convention, language
- change the variable names that are produced by g2ctl
- in the conversion to netcdf, you may want a different set of variable names

### Caution

The user-defined grib tables allow you to use your own names for variables.
However, you have to be careful. Suppose you want to define a new variable
name and you didn'r realize the name was already defined for another variable.
Then the inventory could be confusing and the results of -set_var would be
unpredictable.

The option, -ncep_uv, combines the UGRD and VGRD fields into one
grib message. This option will not work when UGRD and VGRD are renamed.

The option, -new_grid, does vector interpolation of vector fields and
scalar interpolation on the other fields fields. The -new_grid option will
not identify the vector fields if their names get changed and scalar
interpolations will be used. The identification of maxx and wind fields
in the Arakawa E grid will also not work.

### Add a grib table to wgrib2

To add a grib table to the wgrib2 source code, you need to send a copy of
the grib table in the format shown above to wesley.ebisuzaki@noaa.gov. The
advantage is that you get to test the grib table as a "user defined gribtable"
before finalizing the table. You can submit that grib table as

1. for only the locally defined variables

- for all the variables (local and WMO defined)

---

> Description: user grib2tables

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/user_grib2tables.html>_
