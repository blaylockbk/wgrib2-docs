# wgrib2: -elseif

## Introduction

The -elseif option is part of the
-if,
-else,
-endif structure for conditional execution of wgrib2 options.
The -elseif option must follow an
-if or -elseif type option, and it must preceed either the next
-elseif-type,
-else or
-endif option.

The -elseif option is a test for a regular expression.
For a non-regex string, use the -elseif_fs option.

More details are in the [-if](./if.html) documentation.
The -else option was introduced with wgrib2 v3.0.0 to replace
the older version 1 IF structure.

See also:
[-if](./if.html),
[-if_fs](./if_fs.html),
[-else](./else.html),
[-elseif_fs](./elsefs_fs.html),
[-endif](./endif.html),
[-match](./match.html),

---

> Description: elif X elseif X (POSIX regular expression) conditional on match, -if ... -elseif ... -endif

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/elseif.html>_
