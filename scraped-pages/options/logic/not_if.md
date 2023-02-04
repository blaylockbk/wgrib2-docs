# wgrib2: -not_if

## Introduction

The -not_if option returns true or false and is the start of the IF
block structure. The
-not_if option is like the
-if option except the results are reversed.
For example,
-if ':HGT:' is true if the match inventory contains the string ':HGT:', and
the next block is executed. The option -not_if ':HGT:' does the opposite
and does not execute the next block.
See [-if](./if.html) for details about the IF block structure.

## Usage

```

-not_if X
   X is a regular expression
   returns true if X is not in the match inventory, false otherwise

```

See also: [-not](./not.html),
[-end](./end.html),
[-i](./i.html),
[-if](./if.html),
[-if_fs](./if_fs.html),
[-match](./match.html),
[-match_inv](./match_inv.html),
[-not_if_fs](./not_if_fs.html),
[-fi](./fi.html).
[-endif](./endif.html).
[-else](./else.html).
[-set_regex](./set_regex.html).

---

> Description: if X not_if X (regular expression), conditional execution on not match

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/not_if.html>_
