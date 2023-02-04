
### wgrib2: -match



### Introduction



The -match option selects records which should 
be processed. When multiple -match options are used, all matches 
must be satisfied. The -match and
-not options seem to be similar to the
-if and -if\_not options.
The big difference is that the 
-match and -not 
options are processed before any other record processing. If
the record satisfies the -match and 
-not options, then the record is
processed. This include the optional decoding and latitude-longitude 
calculation and the other options.



```


    wgrib2 -match X (...)

is the same as 

    wgrib2 -match_inv file | egrep X | wgrib2 -i (...)



    wgrib2 -match X -match Y -not Z (...)

is the same as 

    wgrib2 -match_inv file | egrep X | egrep Y | egrep -v Z | wgrib2 -i (...)

where X, Y and Z are regular expressions. 

If X, Y and Z are "fixed strings" rather than regular expressions, 
use -match_fs, and -not_fs.

```

### Usage




```

-match X

X is a posix extended regular expression

```


The -match, and -not selection
facility is more limited than the "wgrib2 | filter | wgrib2 -i" syntax.
However, it can be more efficient especially when combined with the 
-end option. Note that the "match" inventory
often expands. Usually the inventory expands by adding new items
to the end of the inventory in order not the break scripts.

### Examples



```

wgrib2 IN.grb -match ":(UGRD|VGRD|TMP):(200|500) mb:"

selects the UGRD, VGRD and TMP fields at the 200 and 500 mb levels

```

### -match vs -if


 The 
-match, and -if can
be confused.
The 
-match option selects the fields that are to be
processed by the command line.
The 
-if option selects the fields that will be processed
and the selection ends at the next
-fi or output option. For example,


```

1. wgrib2 IN.grb -match ":UGRD:200 mb:anl:" -csv u200.csv
2. wgrib2 IN.grb -if ":UGRD:200 mb:anl:" -csv u200.csv

```


Lines 1 and 2 will produce the same CSV file. However, line 1 will only 
process one field. For line 1, only only one field will be docoded
and converted to a CSV file. For line 2, all the fields will be
processed and only one field will be converted to a CSV file. The
total processing will be the docoding of all the fields in the file
and one conversion to a CSV file. 


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

* adding the "extended name of the variable", ex. TMP.prob\_<273
* adding the inventory number, ex. n=10
* adding ensemble/chemical/probability information (-misc)



See also: [-not](./not.html), 
[-not\_fs](./not_fs.html), 
[-match\_fs](./match_fs.html), 
[-match\_inv](./match_inv.html), 
[-end](./end.html), 
[-i](./i.html),
[-if](./if.html),
[-not\_if](./not_if.html),
[-set\_regex](./set_regex.html).












----

>Description: init  X      process data that matches X (POSIX regular expression)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/match.html>_