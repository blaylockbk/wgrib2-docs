# -elseif_rec

## Introduction

The -elseif_rec option is part of the
-if,
-else,
-endif structure for conditional execution of wgrib2 options.
The -elseif type option must follow an
-if or -elseif type option, and it must preceed either the next
-elseif type,
-else or
-endif option.

The -elseif_rec is the
-elseif equivalent of the
-if_rec option.

More details are in the [-if](./if.md) documentation.
The -else option was introduced with wgrib2 v3.0.0 to replace
the older version 1 IF structure.

See also:
[-if](./if.md),
[-if_fs](./if_fs.md),
[-if_n](./if_n.md),
[-if_rec](./if_rec.md),
[-else](./else.md),
[-elseif](./elseif.md),
[-elseif_fs](./elseif_fs.md),
[-elseif_n](./elseif_n.md),
[-endif](./endif.md),
[-match](./match.md),

---

> Description: elif X elseif (record numbers in range), X=(start:end:step)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/elseif_rec.html>_
