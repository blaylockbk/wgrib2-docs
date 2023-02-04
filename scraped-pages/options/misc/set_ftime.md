
### wgrib2: -set\_ftime



### Introduction


 The wgrib2 inventory has a fragment that looks like


```

   :d=2014122500:12 hour fcst:vt=2014122512

   d=2014122500 is the Reference time, usually the analysis or start of the forecast time.
   12 hour fcst is the "ftime", the time is 12 hours after the reference time
   vt=2014122512 is the verification time (ref time + 12 hours)

```


Wgrib2 has options to change the reference time (-set\_time) and to set the ftime.
The -set\_ftime v2 (introduced with wgrib2 v2.0.7) is easier to use and
handles more Product Definition Templates. For compatibility, -set\_ftime v1
is still available on wgrib2 v2.0.7+.


To change the ftime, you add the option -set\_ftime FTIME. What are the allowed formats
of FTIME? The formats are the same as ftimes from a wgrib2 inventory.


```

Original: :d=2014113018:6 hour fcst:
options:  -set_date 2014122500 -set_ftime "12 hour fcst"
New:      :d=2014122500:12 hour fcst:


Original: :d=2014113018:6 hour fcst:
options:  -set_date 2014122500 -set_ave "0-6 hour ave fcst"
New:      :d=2014122500:0-6 hour ave fcst:

Original: :d=2014113018:6 hour fcst:
options:  -set_date 2014122500 -set_ave "124@6 hour ave(0-6 hour ave fcst),missing=0"
New:      :d=2014122500:124@6 hour ave(0-6 hour ave fcst),missing=0:

After changing the time stamp, you can save the file with -grib NEW.grb

```


With wgrib2 v2.0.6 or earlier, you have to use -set\_ave and -set\_ftime.
to convert betweent PDTs. Only 
a subset of the conversions are supported.

 It was noted that the old ftime had problems with the more complicated time stamps and
ftime2 was developed to be an eventual replacement. Similarily set\_ftime2 was developed
to replace the old set\_ftime/set\_ave options.


See also: 












----

>Description: misc  X      either set_ftime1 or set_ftime2 dep on version_ftime

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_ftime.html>_