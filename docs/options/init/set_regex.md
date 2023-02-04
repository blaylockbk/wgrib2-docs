# wgrib2: -set_regex

## Introduction

One common feature of many Unix/Linux commands is the support of
regular expressions (regex). Several of wgrib2's options support
regex (-if, -not_if, -match, -not, -egrep, -egrep_v) which make the various string
comparisons much more versatile (compare egrep vs windows find command).
The -set_regex option changes the flavor
of regex evaluation.

NOTE: the -set_regex option MUST preceed the -match/-not/-if/-not_if/-egrep/-egrep_v options. All these
options are "setup" options and the processing the the -set_regex must occur before the
processing of the -match and other regex options.

Grib files can have hundreds of records and most people only want a few of
the records. Rather than processing everything, you can select the
specific records to process by the -match,
-not,
-if and -not if options. These options
take an extended POSIX regular expression (regex) as their sole argument. These
options check the regex with the "match inventory" (see -match_inv).
Some examples are

```

  wgrib2 input.grb -match ':UGRD:200 mb:' -grib u.grb
  wgrib2 input.grb -match ':(UGRD|VGRD|TMP):200 mb:' -grib uvt.grb

```

Now regex are powerful but can produce some surprises. For example, you want
the 19th (positive perturbation) ensemble member which is denoted by
'ENS=+19' in the match inventory. You try,

```

  wgrib2 input.grb -match ':ENS=+19:' -grib e19.grb

```

Surprise, the above line does not work. The plus sign is a regex
metacharacter indicating that the previous character would be
matched 1 or more times. Consequently the plus sign wouldn't
be matched. To get the above match to work, you can quote the plus sign
with a backslash.

```

  wgrib2 input.grb -match ':ENS=\+19:' -grib e19.grb

```

Alternatively you could change the regex match into fixed-string mode.
In fixed-string mode, the regex metacharacters are considered to be
ordinary characters.

```

  wgrib2 -set_regex 1 input.grb -match ':ENS=+19:' -grib e19.grb

```

Most of the regex options have a fs (fixed string) version,
such as -match and -match_fs.
It is better to used the fixed-string versions are equivalent to
the regex mode set to "fixed string". The fixed string versions
were added because regex library may be unavaible on non-POSIX
systems.

```

  wgrib2 input.grb -match_fs ':ENS=+19:' -grib e19.grb

```

The third mode is the metacharacters have to be quoted.
Here is an example that gets the 19th, 20th and
21th ensemble members. This mode was added because it is easier to remember
to quote the '(|)' metacharacters than to quote the ordinary characters
correspond to metacharacters.

```

  wgrib2 -set_regex 2 input.grb -match ':ENS=+\(19\|20\|21\):' -grib e19_20_21.grb

```

## Usage

```

-set_regex X              X=0, 1, 2
                            0 = extended regular expressions
                            1 = fixed string mode, no metacharacters
                            2 = metacharacters need to be quoted

```

### Example

```

$ wgrib2 gep.grb -match ':UGRD:200 mb:' -match ':ENS=+19:'
        (no output)
$ wgrib2 gep.grb -set\_regex 1 -match ':UGRD:200 mb:' -match ':ENS=+19:'
4.1:86046:d=2009060500:UGRD:200 mb:180 hour fcst:ENS=+19

```

See also:

[-egrep](egrep.html)
[-egrep_v](egrep_v.html)
[-if](if.html)
[-if_fs](if_fs.html)
[-match](match.html),
[-match_fs](match_fs.html),
[-not](not.html),
[-not_fs](not_fs.html),
[-not_if](not_if.html)
[-not_if_fs](not_if_fs.html)

---

> Description: init X set regex mode X = 0:extended regex (default) 1:pattern 2:extended regex & quote metacharacters

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_regex.html>_
