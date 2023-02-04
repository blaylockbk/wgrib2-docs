
### wgrib2: error messages



\*\*\* FATAL ERROR: unknown option -xyz \*\*\*

```

bash-4.1$ wgrib2 small.grb -nosuchoption

*** FATAL ERROR: unknown option -nosuchoption ***

```


In this case, there is no option, -nosuchoption, and wgrib2 rightly complains.
Sometimes options may not exist in older versions of wgrib2.
Sometimes options will be available in the next public release. To see
the list of options that are available with your installed version, type wgrib2 -help all. By the
way, the grib file must not start with a dash or minus sign otherwise
it will be confused with an option. 


xyz package not installed

Wgrib2 has several optional packages. You will get this error message
if you are requesting an option from a package that was not enabled
at compile time.


```

bash-4.1$ wgrib2 small.grb -mysql host user password db table
mysql package not installed

```

 \*\*\* FATAL ERROR: missing arguments option=ij \*\*\*


Every option requires a fixed number of arguments. You will get
this error message when you last option has too few arguments. 
In this case, -ij requires two arguments


```

 bash-4.1$ wgrib2 small.grb -ij 19 

*** FATAL ERROR: missing arguments option=ij ***

```

 \*\*\* FATAL ERROR: too many grib files .. 1st=small.grb 2nd=11 \*\*\*
 
Every option requires a fixed number of arguments. If you have too many or
too few arguments, you can get this error message. In the following
example, -ij, requires two arguments. For the first -ij, the arguments
are 10 and -ij. The "11" doesn't start with a dash, so it is assumed to
be a grib file. Since there was already a grib file found, error message.


```

bash-4.1$ wgrib2 small.grb -ij 10 -ij 11 12

*** FATAL ERROR: too many grib files .. 1st=small.grb 2nd=11 ***

```

grib1 message ignored (use wgrib)

If wgrib2 encounters a grib1 message, it will print the above warning.
Some ECMWF files will include both grib1 and grib2 data. Some
decoders can handle both formats at the same time, but wgrib2 is
not one of them.


```

bash-4.1$ wgrib2 z500.grib1
grib1 message ignored (use wgrib)

```

 \*\*\* FATAL ERROR: missing input file non-existant-file \*\*\*

You get this error message you wgrib2 cannot open the grib file.


```

bash-4.1$ wgrib2 non-existant-file

*** FATAL ERROR: missing input file non-existant-file ***

```

\*\*\* FATAL ERROR: no input file defined \*\*\*


You get this error message when wgrib2 cannot find the input grib file on
the command line. This usually happens when the preceeding option doesn't
have enough arguments. In this example, -ij requires two arguments. The
grib file becomes the second argument.


```

bash-4.1$ wgrib2 -ij 10 small.grb2

*** FATAL ERROR: no input file defined ***

```

\*\*\* FATAL ERROR: rdieee: bad header \*\*\* 

Wgrib2 is trying to read a f77-style binary file (-import\_ieee).


```

(header)		4 byte integer, 4*n
(ieee floating point)   grid point 1
(ieee floating point)   grid point 2
...
(ieee floating point)   grid point n
(trailer)		4 byte integer, 4*n

```


However, either the header is missing or the header has an unexpected value.
The size of the record (4\*n) should be the same as the size of the
grib message that was read in.




(No Output)

```

bash-4.1$ wgrib2 open.gs
bash-4.1$ 

```

 If wgrib2 fails to find a grib2 message, nothing is written out to stdout.
If wgrib2 finds a grib1 message, a warning is written to stderr.
























----

>Description: Error Messages

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/problems.html>_