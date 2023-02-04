
### wgrib2: -alarm



### Introduction



The -alarm N option terminates wgrib2 after N seconds (real time).
For example, you may want to use -alarm to kill a
wgrib2 job that running too long on a web server (ex. grib\_filter). 

 The code for -alarm is SVr4, BSD, POSIX-1-2001 and IEEE Std 1003.1-2001
compatible. The code is also supported by the Cygwin system for Windows systems. The 
-alarm will need to be disabled in the makefile for non-compatible systems.
(Change DISABLE\_ALARM=0 to DISABLE\_ALARM=1 in the makefile.)

 The current version of -alarm simply terminates
the process. This may be the appropriate action when wgrib2 is taking too
long. However, this heavy-handed action may not be an ideal action for programs that 
are calling the wgrib2 subroutine. For these situations, the alarm
should be set up by the main program so that a more appropriate action can
be taken.

 The -alarm option is a 
replacement for -limit which limits
the number of (sub)messages which are processed. Jobs on
web servers may hang because of problems with the network
connections which will never trigger the 
-limit option.

### Usage




```

-alarm N
          N is an integer from 0..65536 (ISO C standard)
          N = 0 will remove any pending alarm

```


The -alarm option is a setup/init option. So
the alarm is activated in the setup phase, cannot altered in the data
processing phase and is not removed the finalize phase. (wgrib2 v2.0.8+
will remove the alarm in the finalize phase.)


For Posix systems, the system generates a SIGALRM signal
to the process after N seconds. The default action is to
terminate the process (wgrib2).



See also: [-quit](./quit.html),
[-limit](./limit.html)


















----

>Description: init  X      terminate after X seconds

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/alarm.html>_