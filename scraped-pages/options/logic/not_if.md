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
See [-if](./if.md) for details about the IF block structure.

## Usage

```

-not_if X
   X is a regular expression
   returns true if X is not in the match inventory, false otherwise

```

See also: [-not](./not.md),
[-end](./end.md),
[-i](./i.md),
[-if](./if.md),
[-if_fs](./if_fs.md),
[-match](./match.md),
[-match_inv](./match_inv.md),
[-not_if_fs](./not_if_fs.md),
[-fi](./fi.md).
[-endif](./endif.md).
[-else](./else.md).
[-set_regex](./set_regex.md).

---

> Description: if X not_if X (regular expression), conditional execution on not match

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/not_if.html>_
