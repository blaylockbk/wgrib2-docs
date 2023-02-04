# wgrib2: -i, -i_file

## Introduction

The -i option specifies that wgrib2 should read
STDIN to determine the records to be processed.
The -i_file option is similar except that
wgrib2 reads file a user-specified file to determine the records to be processed.
Some
of the common uses of -i can also be
done with the -match option.

### Slicing and Dicing

Wgrib2 is a program to "slice and dice" grib2 files. Suppose you have a big grib file
but you only want the 2-meter temperature and the precipitation. Rather than fill up
your disk with big files, you can easily extract the required fields.

The first step is to figure what is in the grib file.

```

-sh-2.05b$ wgrib2 test.grb2 -s
1:0:d=2005090200:HGT:1000 mb:60 hour fcst
2:133907:d=2005090200:HGT:975 mb:60 hour fcst
3:263511:d=2005090200:HGT:950 mb:60 hour fcst
4:389058:d=2005090200:HGT:925 mb:60 hour fcst
5:511037:d=2005090200:HGT:900 mb:60 hour fcst
6:630256:d=2005090200:HGT:850 mb:60 hour fcst
7:745505:d=2005090200:HGT:800 mb:60 hour fcst
....
291:37540403:d=2005090200:GPA:1000 mb:60 hour fcst
292:37677072:d=2005090200:GPA:500 mb:60 hour fcst
293:37791941:d=2005090200:5WAVA:500 mb:60 hour fcst

```

Information overload. Lets see if you can find the desired variables.

```

-sh-2.05b$ wgrib2 test.grb2 -s | grep ':TMP:2 m'
265:35107588:d=2005090200:TMP:2 m above ground:60 hour fcst

```

Found the 2-m temperature, can we find the precipitation?

```

-sh-2.05b$ wgrib2 test.grb2 -s | grep ':PRATE:'
260:34814859:d=2005090200:PRATE:surface:54-60 hour fcst

```

Yes. we found the fields. Now we need to combine the above into a
single command using the or option of egrep; i.e., egrep '(A|B)'.

```

-sh-2.05b$ wgrib2 test.grb2 -s | egrep '(:TMP:2 m|:PRATE:)'
260:34814859:d=2005090200:PRATE:surface:54-60 hour fcst
265:35107588:d=2005090200:TMP:2 m above ground:60 hour fcst

```

Now that we have selected the records, we can send the output (inventory) back
into wgrib2 to manipulate.

```

-sh-2.05b$  wgrib2 test.grb2 -s | egrep '(:TMP:2 m|:PRATE:)' | wgrib2 -i test.grb2 -grib small.grb
260:34814859:d=2005090200:PRATE:surface:54-60 hour fcst
265:35107588:d=2005090200:TMP:2 m above ground:60 hour fcst

```

The above command made a grib2 file consisting of the precipation (PRATE) and 2-m temperature (TMP).

```

-sh-2.05b$ ls -l test.grb2 small.grb
-rw-r--r--    1 wd51we   wd5        212429 2006-10-16 15:08 small.grb
-rwxr-xr-x    1 wd51we   wd5      37862776 2006-05-25 15:16 test.grb2

```

As you can see, the new file is much smaller than the original file.

```

-sh-2.05b$  wgrib2 small.grb
1:0:d=2005090200:PRATE:surface:54-60 hour fcst
2:110654:d=2005090200:TMP:2 m above ground:60 hour fcst

```

As expected, the new grib file only has the desired two fields.

### Decoding a Single record

Another use of the -i option is to specify the field to decode.

```

-sh-2.05b$ wgrib2 test.grb2 -s | grep ':PRATE:' | wgrib2 -i test.grb2 -spread field.txt
260:34814859:d=2005090200:PRATE:surface:54-60 hour fcst
-sh-2.05b$ head field.txt
lon,lat,PRATE surface d=2005090200 54-60 hour fcst
0,-90,5e-06
0.5,-90,5e-06
1,-90,5e-06
1.5,-90,5e-06
2,-90,5e-06
2.5,-90,5e-06
3,-90,5e-06
3.5,-90,5e-06
4,-90,5e-06

```

### -i_file

The -i_file MY_FILE option reads the inventory from
file, MY_FILE. The following 3 lines are equivalent.

```

cat FILE.inv | grep UGRD | wgrib2 -i FILE.grb -bin data.bin

cat FILE.inv | wgrib2 -i FILE.grb -match UGRD -bin data.bin

wgrib2 -i_file FILE.inv FILE.grb -match UGRD -bin data.bin

```

## Usage

```

-i
-i_file FILE

```

### Speed

The following command is very common so it incorporated
within wgrib2. This speeds up the operation by eliminating
two program executions and duplicate reads.

```

wgrib2 gribfile | grep "string" | wgrib2 -i gribfile (other options)

is equivalent to

wgrib2 gribfile -match "string" (other options)

```

However, the -i option can be more efficient
when making multiple extractions from a file. For example,

```

wgrib2 gribfile >gribfile.inv
grep "string1" gribfile.inv | wgrib2 -i gribfile (other options)
grep "string2" gribfile.inv | wgrib2 -i gribfile (other options)
grep "string3" gribfile.inv | wgrib2 -i gribfile (other options)

is faster than

wgrib2 gribfile -match "string1" (other options)
wgrib2 gribfile -match "string2" (other options)
wgrib2 gribfile -match "string3" (other options)

```

See also:
[-match](./match.html),
[-not](./not.html),
[-d](./d.html),

---

> Description: init read Inventory from stdin

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/i.html>_
