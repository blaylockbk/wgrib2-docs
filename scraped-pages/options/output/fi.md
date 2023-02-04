# wgrib2: -fi

## Introduction

The -fi option is used by the version 1 IF block.
It is recommened that version 2 of the IF block be used and
-fi no longer be used.

```
Version 1 IF structure:

  -if TEST (non-output options) (output option)

   If the test is false, all the options up to and including
   the next output option are not executed.  The -fi
   option is a null output option.

   If the test is true, the following options are executed


Version 2 IF block:  (wgrib2 v3.0.0+)

  -if TEST (options) -endif
  -if TEST (options) -else (options) -endif
  -if TEST (options) -elseif TEST (options) -endif
  -if TEST (options) -elseif TEST (options) -elseif TEST (options) -endif
  -if TEST (options) -elseif TEST (options) -else (options) -endif

  The "-else (options)" is optional.
  The "-elseif TEST (options)" is optional, and can be repeated may times.


  The new structure can include nested IF blocks.
```

The -endif option is tne new way to close
an IF block (wgrib2 v3.0.0+). Wgrib2 v3.0.0 handles both
the old and new IF blocks. While there is no plans to
remove the old IF blocks, it is recommended that the new IF
blocks be used because the scripts will be easier to read by future
users. Note that the old and new IF blocks cannot be mixed.

See also:
[-if](./if.md),
[-endif](./endif.md),

---

> Description: out depreceated, used in old IF structure

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/fi.html>_
