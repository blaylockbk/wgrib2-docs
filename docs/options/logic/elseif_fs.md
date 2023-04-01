# -elseif_fs

## Introduction

The -elseif_fs option is part of the
-if,
-else,
-endif structure for conditional execution of wgrib2 options.
The -elseif\* option must follow an
-if or -elseif type option, and it must preceed either the next
-elseif type,
-else or
-endif option.

The -elseif_fs is an
-elseif\* options and
does a comparison with a fixed string (fs) instead
of a regular expression as done by the -else_if.

More details are in the [-if](./if.md) documentation.
The -else option was introduced with wgrib2 v3.0.0 to replace
the older version 1 IF structure.

See also:
[-if](./if.md),
[-if_fs](./if_fs.md),
[-else](./else.md),
[-elseif](./elseif.md),
[-endif](./endif.md),
[-match](./match.md),

---

> Description: elif X elseif X (fixed string) conditional execution

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/elseif_fs.html>_
