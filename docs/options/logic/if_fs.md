# wgrib2: -if_fs, -not_if_fs

## Introduction

The -if_fs option is the same as the
the -if option except it takes "fixed strings"
rather than extended regular expressions. The
-not_if_fs is the same as the -not_if option
except it takes "fixed strings".

The "fixed string" options have the advantage that matches do not use the
regex wildcards. For example, "-if 3.0" will match "300" because the period is
a wildcard character that matches any character including a zero.

## Usage

```

-if_fs X
-not_if_fs X

X is a fixed string (not a regular expression)

```

See also: [-if](./if.md),
See also: [-not_if](./not_if.md),
See also: [-not_if_fs](./not_if_fs.md),

---

> Description: if X if X (fixed string), conditional execution on match

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/if_fs.html>_
