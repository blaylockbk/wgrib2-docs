
### wgrib2: -err\_bin, -err\_string, -eof\_bin, -eof\_string



### Introduction



When you want your want to embed wgrib2 in your program, you want to run wgrib2 and
have ways to talk with wgrib2. For example, we set up a pipe between the main program
and wgrib2. Wgrib2 was suppose to send the a decoded grid to the main program. However,
the grid didn't exist (EOF) or something was wrong (ERR). Consequently wgrib2 didn't 
send the grid over the pipe and the main program just waited for data that would never
come. One method is to send an EOF or ERR message so that the main program would
realize something was wrong. For the case that started this, sending a binary 0
was enough to alert the main program. (The main program was expecting a fortran
sequential file and a record size of 0 is not a reasonable value.)


The -err\_bin, -eof\_bin, -err\_string,
and -eof\_string options are setup options. Like all setup options, they
are only evaluated before processing of the file and in the case of duplicate options, 
only the last applies. 
The -err\_bin and -eof\_bin options write integer
to the specified file. The size of the integer will be determined by the size of the native
integer (often a compile option on 64 bit machines). The endian properties will depend
whether the code is compiled a big or little endian machine.


The -err\_\* options will write output when a "FATAL ERROR" message is triggered.
The -eof\_\* options will write output when wgrib2 ends with out a "FATAL ERROR"
message. However, if wgrib2 fails in an abnormal way (example: segmentation fault), 
none of these routines will write output.


### Usage



```

-err_bin file integer
-err_string file string
-eof_bin file integer
-eof_string file string

```




See also:








----

>Description: init  X Y    send string to file upon EOF: X=file Y=string

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/eof_string.html>_