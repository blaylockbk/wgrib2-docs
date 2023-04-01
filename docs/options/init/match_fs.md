# -match_fs, -not_fs, -if_fs, -not_if_fs

## Introduction

The (-match_fs, -not_fs, -if_fs, -not_if_fs) options are
similar to the
(-match, -not, -if, -not_if) options except the former
group uses fixed strings rather than regular expressions.

Now why would you want to use the "fs" versions? Suppose you want to to
search for the "2.5 mb" level.

```
bash-4.1$ wgrib2 -match ":2.5 mb:" junk
1:0:d=2009060500:HGT:225 mb:180 hour fcst:ENS=+19
2:46042:d=2009060500:HGT:2.5 mb:180 hour fcst:ENS=+19
```

What happened? The command matched the 2.5 and 225 mb level! Well the
period is regular expression metacharacter and matchs any character. To
only get "2.5 mb" either have to quote the period or change the regex mode.

```
bash-4.1$ wgrib2 -match ":2\.5 mb:" junk
2:46042:d=2009060500:HGT:2.5 mb:180 hour fcst:ENS=+19

bash-4.1$ wgrib2 -set_regex 0 -match ":2.5 mb:" junk
1:0:d=2009060500:HGT:225 mb:180 hour fcst:ENS=+19
2:46042:d=2009060500:HGT:2.5 mb:180 hour fcst:ENS=+19

bash-4.1$ wgrib2 -set_regex 1 -match ":2.5 mb:" junk
2:46042:d=2009060500:HGT:2.5 mb:180 hour fcst:ENS=+19

bash-4.1$ wgrib2 -set_regex 2 -match ":2.5 mb:" junk
2:46042:d=2009060500:HGT:2.5 mb:180 hour fcst:ENS=+19
```

So setting the regex mode to 1 or 2 will work. The "\_fs" options
uses fixed strings with no metacharacters which is the same
as the regex mode set to 1.

The "\_fs" options only requires standard C support rather than POSIX-2
so they are available on all wgrib2. Options using
regular expressions are optional and may not be available on platforms.

See also:
[-if](./if.md),
[-if_fs](./if_fs.md),
[-match](./match.md),
[-match_inv](./match_inv.md),
[-not](./not.md),
[-not_fs](./not_fs.md),
[-not_if](./not_if.md),
[-not_if_fs](./not_if_fs.md),
[-set_regex](./set_regex.md).

---

> Description: init X process data that matches X (fixed string)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/match_fs.html>_
