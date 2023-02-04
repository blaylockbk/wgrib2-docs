# wgrib2: -if, -not_if

## Introduction IF block structure

The -if option returns either true or false. What happens
next depends on whether you are using Version 1 or Version 2 of the "If blocks".
Prior to wgrib2 v3.0.0, you only get Version 1. With wgrib2 3.0.0+, you can use
either Version 1 or 2 but not both at the same time.

### Version 1 If blocks

For Version 1 "IF blocks", the -if option returns true or false. If true, then
all the options up to and including the next "output" option are executed. If false, then
all the options up to and including the next "output" option are not executed. Options following
the output option are executed as normal.With Version 1, nesting of IF blocks is not defined. The
option -fi is a NULL output option. Note that -fi
cannot be used in Version 2 "IF blocks".

### Definition

```

    wgrib2 (...) [if] [list of non-ouput options] [output option]

   [if] = -if, -not_if, -if_fs, -not_if_fs, -if_n, -if_rec

   if [if] returns the value of false, the list of non-output options
                                       and the next output option are not executed
           returns the value of true, the following options are executed
   [output option] is the terminator of the if block

   If blocks should not be nested. The results are undefined.

```

### Example

```

     wgrib2 gribfile -if ":(UGRD|VGRD):" -grib winds.grb \
                     -if ":TMP:" -grib tmp.grb \
                     -if ":HGT:" -grib hgt.grb \
                     -not_if ":(UGRD|VGRD|TMP|HGT):" -grib rest.grb

     note: -grib is an output option

     wgrib2 gribfile -new_grid_interpolation bilinear \
        -if ":SOTYP:" -new_grid_interpolation neighbor -fi \
        ...

     note: -fi is an output option

```

### Version 2 IF blocks (introduced wgrib2 v3.0.0)

For Version 2 of the IF blocks, wgrib2 uses
-if, -elseif, -else and
-endif options to implement a proper IF blocking structure. IF blocks
can be nested. Note, the "if" flag is reset prior to processing of a record.

### Definition

```

1. The if blocks can now be nested, and are terminated by an -endif.
2. Within the if block, you can have -else and -elseif options.
3. The -elseif options must proceed the -else option.
4. The -fi option is not allowed.
5. Version 1 and Version 2 if blocks cannot be mixed.

```

### Example 1

```

     wgrib2 gribfile -if ":(UGRD|VGRD):" -grib winds.grb \
                     -elseif ":TMP:" -grib tmp.grb \
                     -elseif ":HGT:" -grib hgt.grb \
                     -else -grib rest.grb -endif

     wgrib2 gribfile -new_grid_interpolation bilinear \
        -if ":SOTYP:" -new_grid_interpolation neighbor -endif \
        ...

```

### Example 2

The SNOHF and Clear Sky radienaces were converted from the nemsio file as
an instantaneous forecast rather an average. (A problem with the nemsio
file.) So I needed to change the
grib files. The new IF blocks make the code look readable. The old
IF blocks would be a mess.

```

# bash: fix SNOHF and CS(radiances) timing info
for f in *.grb
do
  wgrib2 $f \
    -if "(:SNOHF:|:CS)" \
      -if ":3 hour fcst:" -set_ftime "0-3 hour ave fcst" -endif  \
      -if ":6 hour fcst:" -set_ftime "3-6 hour ave fcst" -endif  \
      -if ":9 hour fcst:" -set_ftime "6-9 hour ave fcst" -endif \
      -if ":12 hour fcst:" -set_ftime "9-12 hour ave fcst" -endif \
      -if ":15 hour fcst:" -set_ftime "12-15 hour ave fcst" -endif \
    -endif \
    -grib ../test/$f
done

```

### Limitations

The Version 1 limits,
the maximum number of -if options on a command
line is limited by (1) system limit of open files
(2) maximum length of a command line,
(3) maximum number of regular expressions allowed MAX_MATCH (wgrib.h),
and (4) maximum number of parsed arguments N_ARGLIST (wgrib.h).

The Version 2 limits include the Version 1 limits and include a limit
of the number of nested IF blocks (10).

Version 1 and Version 2 IF blocks cannot be mixed. The
-fi option is restricted to Version 1 IF blocks.
There are NO plans to eliminate Version 1 IF blocks. However,
Version 2 IF blocks are recommended for future development.

## Introduction -if and -not_if

## Usage

```

-if X

X is a regular expression
sets the "run_flag" if X matches the "match inventory",
used to control the execution of following options

```

### Future Changes

The format of the "match inventory" has evolved and will continue to evolve.
The rule for future changes is that new items in the "match inventory" will be added
as the second last item. Consequently the last item in the inventory will always
be ":vt=YYYYMMDDHH:". In order to future proof your
-match, and -not selections, you
must not include any item before the ":vt=YYYYMMDD:" field.

```

    -match ":vt=2011111500:"                  good
    -not ":vt=2011111500:$"                   good (dollar sign matches the end of the line)
    -not ":n=10:vt=2011111500:"               bad (item before :vt=)
    -match ":RH:975 mb:anl::vt=2010050806:"   bad (item before :vt=)

```

Some recent changes (as of Nov 2011) to the match inventory include:

- adding the "extended name of the variable", ex. TMP.prob\_<273
- adding the inventory number, ex. n=10
- adding ensemble/chemical/probability information (-misc)

See also: [-not](./not.md),
[-end](./end.md),
[-match](./match.md),
[-i](./i.md),
[-if](./if.md).
[-if_fs](./if_fs.md).
[-fi](./fi.md).
[-set_regex](./set_regex.md).

---

> Description: if X if X (POSIX regular expression), conditional execution on match

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/if.html>_
