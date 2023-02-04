# wgrib2: -ndates_fmt (v2.0.8+)

## Introduction

The -ndates_fmt option changes the default format
that the -ndates prints the date codes. The default
format is written in C as " %s". (The date code is converted into a string,
and a blank and the string is printed out.)

The -ndates_fmt option is an initialization option,
so it runs prior to the processing of the grib file.
The -ndates_fmt option needs to preceed the
-ndates option.

```

Default ndates format is " %s"

$ wgrib2 /dev/null -ndates 201802 1dy 6hr
 2018020100 2018020106 2018020112 2018020118$

A list of files on one line

$ wgrib2 /dev/null -ndates_fmt " pgb%s" -ndates 201802 1dy 6hr
 pgb2018020100 pgb2018020106 pgb2018020112 pgb2018020118$

A list of files, one file per line

$ wgrib2 /dev/null -ndates_fmt "pgb%s\n" -ndates 201802 1dy 6hr
pgb2018020100
pgb2018020106
pgb2018020112
pgb2018020118
$

Making a script to process a list of files

bash-4.1$ wgrib2 /dev/null -ndates_fmt "cp pgb%s ~/data\n" -ndates 201802 1dy 6hr >cmd
bash-4.1$ cat cmd
cp pgb2018020100 ~/data
cp pgb2018020106 ~/data
cp pgb2018020112 ~/data
cp pgb2018020118 ~/data
bash-4.1$

```

The -ndates_fmt option understands three back-slash characters.

```

   \n gets converted into a new-line character
   \t gets converted into tab character
   \\ gets converted into a back-slash character

   Windows will be addressed later with respect to the end of line termination.

```

## Usage

```

-ndates_new C_FORMAT
   C_FORMAT is a C-language format which includes a %s to print
   the date code.  The only back slash sequences allowed are \n, \t, and \\.

```

See also: [-ndates](./ndates.html)

---

> Description: init X X = C format for ndates option ex. 'date=%s'

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ndates_fmt.html>_
