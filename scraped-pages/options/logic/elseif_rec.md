# wgrib2: -elseif_rec

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

More details are in the [-if](./if.html) documentation.
The -else option was introduced with wgrib2 v3.0.0 to replace
the older version 1 IF structure.

See also:
[-if](./if.html),
[-if_fs](./if_fs.html),
[-if_n](./if_n.html),
[-if_rec](./if_rec.html),
[-else](./else.html),
[-elseif](./elseif.html),
[-elseif_fs](./elseif_fs.html),
[-elseif_n](./elseif_n.html),
[-endif](./endif.html),
[-match](./match.html),

---

> Description: elif X elseif (record numbers in range), X=(start:end:step)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/elseif_rec.html>_
