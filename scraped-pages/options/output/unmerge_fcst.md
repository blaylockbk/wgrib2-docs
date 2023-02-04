
### wgrib2: -unmerge\_fcst (v3.0.0+)



### Introduction


Prior to GFS-FV3, the precipitation was stored in the "old-style". This style
handled 6 hour accumulations in a simple manner.


```

  0-6, 6-12, 12-18, 18-24, etc

```



Eventually someone wanted to store 3 hour accumulations. However, people who had code
that read the 6 hour accumulations said that they couldn't change the code. So the
the file still had to contain the 6 hour accumulations. So the code to read the
3 hour accumulations had to a subtraction for some of the interval. They stored
the accumulations like this.



```

  0-3, 0-6, 6-9, 6-12, 12-15, 12-18, 18-21, 18-24, etc

```


Now the FV3-GFS model developers agreed to store precipitation accumulations in
the easy-to-use format,


```

  0-3, 0-6, 0-9, 0-12, 0-15, 0-18, 0-21, 0-24, etc

```


However, people who had code that read the accumulation in the old style said that
they could not change their code. So they had put old style and new style accumulated
precipitation in the files.


To process the precip with wgrib2, you want to eliminate the old-style precip.
It is surprisingly difficult because the old style can have to same name as
the new style (ex. 3 hour forecast). You have to eliminate the old style but
not the new style even though they have the same name.


 The -unmerge\_fcst option helps deal with the
FV3-GFS precip problem when trying to make time series.


### Example usage



```

# make a file with the APCP from cat gfs.t00z.pgrb2.0p25.f(N) N=000..029 
$ cat gfs.t00z.pgrb2.0p25.f00? gfs.t00z.pgrb2.0p25.f01? gfs.t00z.pgrb2.0p25.f02? | wgrib2 - -match APCP -grib $stmp/all_apcpc.grb


# Lets see the new-style precip. in the file
$ cd $stmp
$ wgrib2 all_apcpc.grb -match ':0-'
1:0:d=2020101600:APCP:surface:0-1 hour acc fcst:
2:225707:d=2020101600:APCP:surface:0-1 hour acc fcst:
3:451414:d=2020101600:APCP:surface:0-2 hour acc fcst:
4:726257:d=2020101600:APCP:surface:0-2 hour acc fcst:
5:1001100:d=2020101600:APCP:surface:0-3 hour acc fcst:
6:1307459:d=2020101600:APCP:surface:0-3 hour acc fcst:
7:1613818:d=2020101600:APCP:surface:0-4 hour acc fcst:
8:1940346:d=2020101600:APCP:surface:0-4 hour acc fcst:
9:2266874:d=2020101600:APCP:surface:0-5 hour acc fcst:
10:2610484:d=2020101600:APCP:surface:0-5 hour acc fcst:
11:2954094:d=2020101600:APCP:surface:0-6 hour acc fcst:
12:3311989:d=2020101600:APCP:surface:0-6 hour acc fcst:
14:3894753:d=2020101600:APCP:surface:0-7 hour acc fcst:
16:4539821:d=2020101600:APCP:surface:0-8 hour acc fcst:
18:5222224:d=2020101600:APCP:surface:0-9 hour acc fcst:
20:5934324:d=2020101600:APCP:surface:0-10 hour acc fcst:
22:6672253:d=2020101600:APCP:surface:0-11 hour acc fcst:
24:7431863:d=2020101600:APCP:surface:0-12 hour acc fcst:
26:8071608:d=2020101600:APCP:surface:0-13 hour acc fcst:
28:8767119:d=2020101600:APCP:surface:0-14 hour acc fcst:
30:9496847:d=2020101600:APCP:surface:0-15 hour acc fcst:
32:10253719:d=2020101600:APCP:surface:0-16 hour acc fcst:
34:11034026:d=2020101600:APCP:surface:0-17 hour acc fcst:
36:11835057:d=2020101600:APCP:surface:0-18 hour acc fcst:
38:12515832:d=2020101600:APCP:surface:0-19 hour acc fcst:
40:13250721:d=2020101600:APCP:surface:0-20 hour acc fcst:
42:14017034:d=2020101600:APCP:surface:0-21 hour acc fcst:
44:14807995:d=2020101600:APCP:surface:0-22 hour acc fcst:
46:15623322:d=2020101600:APCP:surface:0-23 hour acc fcst:
48:16460745:d=2020101600:APCP:surface:0-1 day acc fcst:
50:17171481:d=2020101600:APCP:surface:0-25 hour acc fcst:
52:17933059:d=2020101600:APCP:surface:0-26 hour acc fcst:
54:18724060:d=2020101600:APCP:surface:0-27 hour acc fcst:
56:19538757:d=2020101600:APCP:surface:0-28 hour acc fcst:
58:20377109:d=2020101600:APCP:surface:0-29 hour acc fcst:

```


As you can see, 0-1, 0-2, 0-3, 0-4, 0-5 and 0-6 accumulations are duplicated.

The -unmerge\_fcst option is used to find the precipitation time series.


```

$ wgrib2 all_apcpc.grb -unmerge_fcst precip_ts.grb 0hr 1
1:0:d=2020101600:APCP:surface:0-1 hour acc fcst:
2:225707:d=2020101600:APCP:surface:0-1 hour acc fcst:
3:451414:d=2020101600:APCP:surface:0-2 hour acc fcst:
4:726257:d=2020101600:APCP:surface:0-2 hour acc fcst:
5:1001100:d=2020101600:APCP:surface:0-3 hour acc fcst:
6:1307459:d=2020101600:APCP:surface:0-3 hour acc fcst:
...

```


The first argument is the output file. The second argument is
the start of the accumulations to start, i.e., 0hr. Finally the
third argument is 1 to include the 0-1 accumulation. The output is


```

$ wgrib2 precip_ts.grb 
1:0:d=2020101600:APCP:surface:0-1 hour acc fcst:
2:225707:d=2020101600:APCP:surface:1-2 hour acc fcst:
3:2302390:d=2020101600:APCP:surface:2-3 hour acc fcst:
4:2302593:d=2020101600:APCP:surface:3-4 hour acc fcst:
5:2302796:d=2020101600:APCP:surface:4-5 hour acc fcst:
,,
26:2307059:d=2020101600:APCP:surface:25-26 hour acc fcst:
27:2307262:d=2020101600:APCP:surface:26-27 hour acc fcst:
28:2307465:d=2020101600:APCP:surface:27-28 hour acc fcst:
29:2307668:d=2020101600:APCP:surface:28-29 hour acc fcst:

```

### Usage




```

-unmerge_fcst (output file) (N)(time unit) I
   output file: name of output grib file
   N: integer,  usually 0
   time unit:  the forecast time unit, hr, dy, mo, etc
      If you want to process A-B hour acc fcst
      then N=1, time unit = hr
  I = 1 

```


See also: 














----

>Description: out   X Y Z  unmerge_fcst X=output Y=fcst_time_0 Z: 0->result 1->+init 2->+all

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/unmerge_fcst.html>_