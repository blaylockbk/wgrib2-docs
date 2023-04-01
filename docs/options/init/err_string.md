# -err_bin, -err_string, -eof_bin, -eof_string

## Introduction

When you want your want to embed wgrib2 in your program, you want to run wgrib2 and
have ways to talk with wgrib2. For example, we set up a pipe between the main program
and wgrib2. Wgrib2 was suppose to send the a decoded grid to the main program. However,
the grid didn't exist (EOF) or something was wrong (ERR). Consequently wgrib2 didn't
send the grid over the pipe and the main program just waited for data that would never
come. One method is to send an EOF or ERR message so that the main program would
realize something was wrong. For the case that started this, sending a binary 0
was enough to alert the main program. (The main program was expecting a fortran
sequential file and a record size of 0 is not a reasonable value.)

The -err_bin, -eof_bin, -err_string,
and -eof_string options are setup options. Like all setup options, they
are only evaluated before processing of the file and in the case of duplicate options,
only the last applies.
The -err_bin and -eof_bin options write integer
to the specified file. The size of the integer will be determined by the size of the native
integer (often a compile option on 64 bit machines). The endian properties will depend
whether the code is compiled a big or little endian machine.

The -err\_\* options will write output when a "FATAL ERROR" message is triggered.
The -eof\_\* options will write output when wgrib2 ends without a "FATAL ERROR"
message. However, if wgrib2 fails in an abnormal way (example: segmentation fault),
none of these routines will write output.

If a "FATAL ERROR" occurs before the The -err\_\* option is initialized, there will
be no error file written. This can occur when the command line options are wrong
(option expects 3 arguments and you supply a different number). It can also
occur when the initialization of an earlier option failed. (Ex. read from a
non-existant file.) Consequently the -err\_\* options should be in front of the
comamand line arguments.
right.

## Usage

```
-err_bin file integer
-err_string file string
-eof_bin file integer
-eof_string file string
```

See also:

---

> Description: init X Y send string to file upon err exit: X=file Y=string

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/err_string.html>_
