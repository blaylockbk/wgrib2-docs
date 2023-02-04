# pywgrib2_s: calling wgrib2 subroutine

## Introduction

Wgrib2 is a program that can be called called using various computer languages.
This provides an easy way to read and create grib files.

- shell: wgrib2 GFS.grb -match ':HGT:200 mb:' -netcdf hgt200.nc
- C: i = system("wgrib2 GFS.grb -match ':HGT:200 mb:' -netcdf hgt200.nc");
- Fortran: call execute_command_line('wgrib2 GFS.grb -match '':HGT:200 mb:'' -netcdf hgt200.nc')
- perl: i = system "wgrib2 GFS.grb -match ':HGT:200 mb:' -netcdf hgt200.nc"
- python: os.system("wgrib2 GFS.grb -match ':HGT:200 mb:' -netcdf hgt200.nc")

Calling the wgrib2 utility has much overhead (see list below); however, this is a reasonable
procedure if you are not time constrained or you are calling wgrib2 a few times.
Except for second and third items, all the following overheads involve the I/O system. Therefore,
a solution needs to address these overheads.

wgrib2 utility overhead 2. loading the shell and wgrib2 utility for every call to wgrib2 (sometimes avoidable)

- "compiling" the wgrib2 command line
- initialization of wgrib2, "compiling" the wgrib2 command line
- unnecessary opening and closing of files
- all communication of data is through disk files

Making wgrib2 a routine can fix all these overheads except for the second.

callable wgrib2 overhead 2. Fixed: direct subroutine call to wgrib2. No loading shell/wgrib2 utility

- Reduced impact: after first call to wgrib2, initialization of wgrib2 is reduced
- Not fixed: "compiling" the wgrib2 command line
- Fixed: by default, files are not closed between calls to wgrib2
- Fixed: can communicate using memory files and RPN registers

The only overhead that can't be fixed is the initialization and "compiling"
by wgrib2. This overhead does not involve
the I/O system, and becomes relatively small as the grid size increases.

###

```
#!/usr/bin/env python3

# this version does a call to wgrib2 to netcdf files
# does not make or use inventory file

import pywgrib2_s
in_file='gfs.t00z.master.grb2f048'
outa='gfs_1a.nc'
outb='gfs_1b.nc'

err = pywgrib2_s.wgrib2( [in_file, '-rewind_init', in_file, '-match',':HGT:500 mb:', "-netcdf", outa] )
print("write netcdf-a=",err)
err = pywgrib2_s.wgrib2( [in_file, '-rewind_init', in_file, '-match',':TMP:850 mb:', "-netcdf", outb] )
print("write netcdf-b=",err)
```

Usage

```
ierr = pywgrib2_s.wgrib2( [ (list of wgrib2 arguments) ] )
      note: output files remain open and may not be flushed
            previously opened output files will continue writing after last write
            input files remain open
            previously opened input files will continue reading after last read

      note: you can flush the output file by closing the file by pywgrib2_s.close(file)
      note: you can read from the beginning of a file by '-rewind_init', file after the
            file has been opened
```

Version and Configuration of Wgrib2 Routine

```
result = pywgrib2_s.wgrib2_version()
         result is a string with the wgrib2 version (same as wgrib2 -version)

result = pywgrib2_s.wgrib2_configuration()
         result is a list of strings
         the same as wgrib2 -config
```

How it works

John Howard (2014) submitted changes that
optionally make wgrib2 a routine rather than a program.
The arguments to the wgrib2 routine were the same as the arguments to the wgrib2
the program. Since wgrib2 is written in C, the arguments to
wgrib2 are the length of the array of strings followed by
an array of pointers to strings. Being a C program, the strings
are terminated by a zero.

The pywgrib2_s loads a shared wgrib2 library and calls the "wgrib2"
using the following python code.

```
def wgrib2(arg):
    #
    #    call wgrib2
    #        ex.  pywgrib2.wgrib2(["in.grb","-inv","@mem.0"])
    #
    #    uses C calling convention: 1st arg is name of program
    #
    global debug
    arg_length = len(arg) + 1
    select_type = (c_char_p * arg_length)
    select = select_type()
    item = "pywgrib2"
    select[0] = item.encode('utf-8')

    for key, item in enumerate(arg):
        select[key + 1] = item.encode('utf-8')

    if debug: print("wgrib2 args: ", arg)
    ierr = my_wgrib2.wgrib2(arg_length, select)
    if debug: print("wgrib2 err=", ierr)
    return ierr
```

See also:

---

> Description:

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pywgrib2_s_wgrib2.html>_
