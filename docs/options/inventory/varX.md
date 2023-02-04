# wgrib2: -varX

## Introduction

The -varX option writes a raw variable name. It
shows the discipline, master table, local table, center, parameter category
and parameter number. The default format is

```

var{discipline}_{master table}_{local table}_{center}_{parameter_category}_{parameter}

ex.  var0_10_1_57_1_51
       discipline = 0
       master table = 10
       local table = 1
       parameter category = 1
       parameter = 51

```

When you use verbose mode > 0 (-v, -v2, -v98, -v99), then the format
of -varX changes to

```

(WMO defined variables)

ex. var discipline=0 master_table=2 parmcat=2 parm=3

For locally defined variables, the local table is important and the format is

ex.  var discipline=0 local_table=1 center=7 parmcat=1 parm=195

```

One would use the -varX option when
the built-in tables have a problem. For example, suppose SGP
is a new parameter this is not in your version of wgrib2.
However, you want to write a script that will extract SGP
and work with both the
old and future versions of wgrib2. By using
-varX, your inventories will have a variable
name that is the same in both versions of wgrib2. The varX name
has been added to the match_inventory (2.0.2 3/2015) and is
understood by -set_var and -set_metadata (2.0.7 12/2017).

## Usage

```

-varX

```

### Example

```

$ wgrib2 test.grb2 -varX
1:0:var0_2_1_7_3_5

```

See also: [-var](./var.md)

---

> Description: inv raw variable name - discipline mastertab localtab center parmcat parmnum

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/varX.html>_
