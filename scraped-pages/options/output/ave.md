
### wgrib2: -ave, -fcst\_ave



![News](../icons/new.png)
 
With wgrib2 v2.0.7, 
the -ave and -fcst\_ave 
now call the -time\_processing option.
This option handles more statistical operations and more 
Product Definition Templates (PDT).

The old -ave and -fcst\_ave options will available as
-ave0 and -fcst\_ave0. I expect that
these two options will be eliminated within a year of the release of v2.0.7. 


### Introduction



The -ave 
and -fcst\_ave options are very similar;
they both make temporal averages.
The -fcst\_ave option assumes that the
reference (initial) time is constant and the verification time
is increasing.
The
-ave option assumes the reference time
is increasing and the difference between the verification and
reference time is constant.



You would use -fcst\_ave to temporally average
a single forecast run. For example, you have a 3 week forecast with
output every 6 hours. You could use -fcst\_ave 
to find the forecast for the second week.


You would use -ave to temporally average
several analyses. Suppose you have
analyses every 6 hours and you want to find the analysis for the month.


The input grib file has to be processed in a special order. Don't worry, 
a grib file can be ordered very easily with the sort command. wgrib2 reads the data
sequentially and when ever it encounters a new variable/level/chemical-type,
it starts the averaging process. The length of the averaging depends on
how many records it finds to average. For example, to make a daily
average, a file has to be in the following order.


```

U500 2000-01-02 00Z             start ave
U500 2000-01-02 06Z
U500 2000-01-02 12Z
U500 2000-01-02 18Z             end ave
V500 2000-01-02 00Z             start ave
V500 2000-01-02 06Z
V500 2000-01-02 12Z
V500 2000-01-02 18Z             end ave
Z500 2000-01-02 00Z             start ave
Z500 2000-01-02 06Z
Z500 2000-01-02 12Z
Z500 2000-01-02 18Z             end ave

```


To make a daily average of the above file, you need to specify the
output file and the time interval between samples. The time
units are the same as used by GrADS (hr, dy, mo, yr).


```

$ wgrib2 input.grb -ave 6hr out.grb

```



If the file is not sorted, you can use the unix sort by,


```

$ wgrib2 input.grb | sort -t: -k4,4 -k5,5 -k6,6 -k3,3 | \
 wgrib2 -i input.grb -set\_grib\_type c3 -ave 6hr output.grb

```


If you want to make daily means from 4x daily monthly files
and assuming that more than one variable/level is in the monthly file.


```

$ wgrib2 input.grb | sed 's/\(:d=........\)/\1:/' | \
 sort -t: -k3,3 -k5,5 -k6,6 -k7,7 -k4,4 | \
 wgrib2 input.grb -i -set\_grib\_type c3 -ave 6hr daily.ave.grb

```


Using -fcst\_ave is like using 
-ave except you use the verification
time instead of the reference time. To make an inventory that
use the verification time instead of the reference time, you type, 


```

$ wgrib2 input.grb -vt -var -lev -misc 
1:0:vt=2011040101:PRATE:surface:
2:592224:vt=2011040102:PRATE:surface:
3:1233694:vt=2011040103:PRATE:surface:
4:1909322:vt=2011040104:PRATE:surface:
5:2612620:vt=2011040105:PRATE:surface:

```


The sed command will be alterered very slightly when making the
sort (:d=) -> (:st=).

### Averaging several files using gmerge


 If want to average several grib files, and the files have the 
following properties:

1. No submessages.
- No non-grib data between the grib messages.
- Each file is for a different time.
- Each file has corresponding grib messages in the same order



```

Conditions 1 and 2 can be met using 

      wgrib2 IN.grb -grib OUT.grb

Condition 4 can be met using 
      wgrib2 IN.grb | sort -k3 -t: | wgrib2 -i IN.grb -grib OUT.grb

```


Then you can use the gmerge program to produce a file in
the correct order. The program gmerge is included with the
wgrib2 distribution under grib2/aux\_progs/.


```

$ ls pgb.20170107??
pgb.2017010700	pgb.2017010706	pgb.2017010712	pgb.2017010718

$ gmerge - pgb.20170107?? | wgrib2 - -ave 6hr /tmp/daily.grb
1:0:d=2017010700:APCP:surface:0-6 hour acc fcst:
2:92905:d=2017010706:APCP:surface:0-6 hour acc fcst:
3:185445:d=2017010712:APCP:surface:0-6 hour acc fcst:
4:278666:d=2017010718:APCP:surface:0-6 hour acc fcst:
5:371535:d=2017010700:ACPCP:surface:0-6 hour acc fcst:
6:442127:d=2017010706:ACPCP:surface:0-6 hour acc fcst:
7:514343:d=2017010712:ACPCP:surface:0-6 hour acc fcst:
8:588096:d=2017010718:ACPCP:surface:0-6 hour acc fcst:
9:661594:d=2017010700:NCPCP:surface:0-6 hour acc fcst:
...

$ wgrib2 /tmp/daily.grb 
1:0:d=2017010700:APCP:surface:4@6 hour ave(0-6 hour acc fcst),missing=0:
2:325115:d=2017010700:ACPCP:surface:4@6 hour ave(0-6 hour acc fcst),missing=0:
3:715210:d=2017010700:NCPCP:surface:4@6 hour ave(0-6 hour acc fcst),missing=0:
...

```

### Fast Averaging several files



Suppose we have a month of analyses at 3 hour intervals and want
to make a monthly mean for Nov. 2014. Using the above sorting approach, the steps
would be


```

1.  cat narr.201411????.grb2 >tmp.grb2
2.  wgrib2 tmp.grb2 |  \
3.     sort -t: -k4,4 -k5,5 -k6,6 -k3,3 | \
4.     wgrib2 tmp.grb2 -i -set_grib_type c3 -ave 3hr narr.201411

The first line creates a file with all the data.
The second line make an inventory.
The third line sorts the inventory in the order for -ave to process.
The fourth line makes the average by processing data in the order
  determined by the inventory created by line 3.

```


The above approach processes one average at a time and requires a
minimal amout of memory. However, if you count the I/O operations,
you find that there are 4 I/O operations for every field as well as
the writes of the monthly means. In addition, the read (line 4) is
random access.


The gmerge approach would look like


```

1.  gmerge - narr.201411????.grb2 | \
2.     wgrib2 - -set_grib_type c3 -ave 3hr narr.201411

The first line creates a file with all the data.
The second line makes the average by processing data from line 1.

```


For this to work, you would have to rewrite gmerge to that it can
large number of input files. For a monthly average of 3-hourly files,
a typical linux system wouldn't have any problems. For a 30-year
climatology, the typical linux system would complain about the
number of open files.

 The number of I/O operations with the gmerge is quite good,
every input file is read one time. The read would behave somewhere
between a sequential read and a random read. Another drawback
is the input files much have the data in the same order.


 The third method takes advantage of HPC file systems which
are very fast for sequential reads of large files and terrible for
random-access reads like used in the 
the sort-of-the-index method. Using this method for computing
monthly means from 3-hourly NARR data was taking three quarters of
an hour on a multi-million dollar machine.
The problem was that the file system was optimized for large
sequential reads rather than small random-access reads.
The following shows another approach.



```

1.  cat narr.201411????.grb2 | \
2.     wgrib2 - \
3.        -if_fs ":HGT:200 mb:" -ave 3hr narr.201411 \
4.        -if_fs ":UGRD:200 mb:" -ave 3hr narr.201411 \
5.        -if_fs ":VGRD:200 mb:" -ave 3hr narr.201411 \
6.        -if_fs ":TMP:200 mb:" -ave 3hr narr.201411 

The first line copies the data in chronological order and
   writes it to the pipe.
The second line has wgrib2 read the grib data from the pipe.
The third line selects the Z200 fields and runs the averaging
  option on it.  We are assuming the narr.* fields only have
  one Z200 field and narr.201411???? puts the data into
  chronological order.
Lines 4-6 apply the averaging option to other fields.

```


The above approach computes the mean of Z200, U200, V200 and T200 data 
at the same time with the use of more memory.
The I/O consists of sequential read of all the files and the
writes of the monthly means. The above script only creates
the mean of Z200, U200, V200 and T200 but you could write a 
very long command line and compute the mean of all the fields in
the file. Here are the guts of a 
bash script, fast\_grib2\_mean.sh, which creates and runs the command line.


```

1.  wgrib2 $1 -match_inv | cut -f4- -d: | sed -e 's/:n=.*//' >$tmp
2.  cmd="cat $* | $wgrib2 - -set_grib_type c3 "
3.  while read line
4.  do
5.    cmd="$cmd -if_fs '$line' -ave $dt $out "
6.  done <$tmp
7.  eval $cmd

1. $1 is the first file to average.
   Line 1 creates a file with the field names (minus date codes)
2. cmd is the command line that is being built
3. loop over all the lines in file $tmp
5. generate the "-if_fs/-ave" for the cmd line
   Older versions of the web paged used -if but that caused problems when
   $line included metacharacters such as parentheses.
6. bash syntax to have the while loop read from $tmp
7. run the command line

```


Making the NARR monthly means using the above approach uses large
sequential reads which is optimal for the HPC file system. The run
time went from 3/4 of an hour to maybe a minute.

### Fast Forecast Averaging



The previous shell script was for a fast averaging of many analyses.
Sometimes one want to average several forecasts starting from
the same initial time. An example would producing a week-4 forecast.


```

1.  $wgrib2 $1 -match_inv | cut -f4-5 -d:  >$tmp
2.  cmd="cat $* | $wgrib2 - -set_grib_type c3 "
3.  while read line
4.  do
5.    cmd="$cmd -if_fs '$line' -fcst_ave $dt $out "
6.  done <$tmp
7.  eval $cmd

1. $1 is the first file to average.
   Line 1 creates a file with the name and level for each field
   It is assumed that the name and level is unique in the file.
2. cmd is the command line that is being built
3. loop over all the lines in file $tmp
5. generate the "-if_fs/-fcst_ave" for the cmd line
   Older versions of the web paged used -if but that caused problems when
   $line included metacharacters such as parentheses.
6. bash syntax to have the while loop read from $tmp
7. run the command line

```

 
Using the -merge\_fcst option in a like
manner to the -fcst\_ave option. in a like



### Monthly Climatologies



Once you can make an average, making a monthly climatology should be easy. Except
it isn't. Here are some of the problems that I encountered.

1. February has 28 days except when it doesn't. This causes problems because
 wgrib2 -ave will not average 28 and 29 day intervals.
- '116@6 hour ave(anl)' includes a regex metacharacter
- the process id changed
- the subcenter changed



The solutions were:

1. rewrite the grib file with   

 -if\_fs '116@6 hour ave(anl)' -set\_ftime2 '112@6 hour ave(anl)' -fi \  

 -if\_fs '116@6 hour ave(6 hour fcst)' -set\_ftime2 '112@6 hour ave(6 hour fcst)' -fi \  

 -if\_fs '116@6 hour ave(3-6 hour acc fcst)' -set\_ftime2 '112@6 hour ave(3-6 hour acc fcst)' -fi \  
- Use -if\_fs instead of -if
- rewrite the file with -set analysis\_or\_forecast\_process\_id 180
- rewrite the file with -set subcenter 0


 
Finding items 3 and 4 was a pain. Using undocumented option v98 helps. However, the mismatches can
now be uncoverted by using a verbose mode (>0) and wgrib v2.0.6.


###  Limitations by wgrib2 version



There is a limit in the maximum number of -if\_fs/-ave clauses. 
Wgrib2 v2.0.6 can process up to 2000 -if and 2000 -if\_fs options and
accept 10000 words on the command line. Since each -if\_fs/-ave clause takes 5 
words on the command line and you need to include the name of the
input file, you get a limit of 999 -if\_fs/-ave clauses. To speed up
the code, the evaluation of the -if\_fs options is done in parallel.


### Usage




```

-ave (time interval)  (output grib file)
-fcst_ave (time interval)  (output grib file)

   wgrib2 prior to v2.0.7 only works with PDT=4.0, 4.1 and 4.8
    support for PDT 4.2 and 4.12 by -ave added 7.2016
   wgrib2 v2.0.7 is limited by the -time_processing option

```


See also: 
[-merge\_fcst](./merge_fcst.html), 
[-time\_processing](./time_processing.html)














































----

>Description: out   X Y    average X=time step Y=output v2

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ave.html>_