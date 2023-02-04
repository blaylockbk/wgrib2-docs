# wgrib2: -elseif_reg

## Introduction

The -elseif_reg option is part of the
-if,
-else,
-endif structure for conditional execution of wgrib2 options.
The -elseif type option must follow an
-if or -elseif type option, and it must preceed either the next
-elseif type,
-else or
-endif option.

The -elseif_reg is the
-elseif equivalent of the
-if_reg option.

More details are in the [-if](./if.html) documentation.
The -else option was introduced with wgrib2 v3.0.0 to replace
the older version 1 IF structure.

See also:
[-if](./if.html),
[-if_fs](./if_fs.html),
[-if_n](./if_n.html),
[-if_rec](./if_rec.html),
[-if_reg](./if_reg.html),
[-else](./else.html),
[-elseif](./elseif.html),
[-elseif_fs](./elseif_fs.html),
[-elseif_n](./elseif_n.html),
[-elseif_rec](./elseif_rec.html),
[-endif](./endif.html),
[-match](./match.html),

---

> Description: elif X elseif rpn registers defined, X = A, A:B, A:B:C, etc A = register number

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/elseif_reg.html>_
