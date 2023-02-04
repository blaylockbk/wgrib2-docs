
### wgrib2: special files



### Introduction: Special Files


Wgrib2 has some special files. They are

1. - (negative sign)
- @tmp:STRING where STRING is a alphanumeric string
- @mem:N N=0..19
- Unix/Linux: /dev/null


###  1. - (negative sign)



This special file name is often used by unix/linux utilities to denote either
stdin or stdout. The context will determine whether stdin or stdout is used.
This special file name is often used to pipe data between processes.


```

Example:  wgrib2 IN.grb -set_grid_winds earth -set_grid ncep grid 3  - | \
            wgrib2 - -ncep_uv OUT.grb

          The first wgrib2 regrids the file to ncep grid #3 and uses -
            to send the output to stdout.
          The second wgrib2 combines U and V into the same grib message.
            In this case, the - indicates that the input file is stdin.
          This form is fast because both processes run in parallel and
          there is no temporary disk file.
 
```

###  2. @tmp:STRING (temporary files)



This special file name is used for creating temporary files which will disappear
after the process is finished. Multiple wgrib2 processes can use the same
@tmp:STRING file without conflict. These files are only useful with callable wgrib2.


```

Example:
!  make a temporary inventory file
  i = wgrib2('ds.td.bin', '-inv', '@tmp:abc.inv')
! inquire how many grid point the "150 hour fcst" has
  i = grb2_inq('ds.td.bin', '@tmp:abc.inv', ':150 hour fcst:',npts=j)
  write(*,*) 'error code=',i,' number of grid points=',j

```

###  3. @mem:N (memory files) (alpha)


 @mem:N where N=0..9 are memory files which are stored in 
computer's random-access memory. Memory files are fast but
are limited by the size of the computer's memory.
Memory files can be used
almost everywhere that files can be used (see below for exceptions). 
Memory files are transient; whenever the wgrib2 ends, the memory files
are lost. Memory files can be read/written to disk files prior/after
processing by using the options -mem\_init and -mem\_final. 
For callable wgrib2, the memory files are lost when the calling program finishes.


 Memory files were implimented so that callable wgrib2 could be
used for HPC. For example, decoding can be done by calling a routine puts a 
grib message into a memory file. The wgrib2 subroutine can then
process the memory file and writes the decoded field to either a
memory file or a RPN register.
Finally the program can call another routine
that reads the memory file or RPN register.

 Memory files are often used by CW2 to store grib inventories.
They are also used internally by grep Fortran API (memory file 19).


Memory files can be used to speed up processing. Suppose you
are going to re-order a grib file. Without memory files, you
would create an inventory (sequential read), sort the inventory,
and do a random-access read and a sequential write. With memory
files, you could copy the grib file into memory and then
avoid the slow random-access read.


```

wgrib2 IN.grb | sort -t: -k3.3 -k4,4 -k5,5 | \
   wgrib2 -i IN.grb -grib sorted.grb

wgrib2 IN.grb | sort -t: -k3.3 -k4,4 -k5,5 | \
   wgrib2 -i -mem_init 0 IN.grb @mem:0 -grib sorted.grb

```

###  4. /dev/null



Don't ask your Linux system admin to recover data sent to /dev/null.

###  Options that don't support memory files


1. -AAIG
- -csv
- -err\_bin
- -err\_bin
- -grib\_ieee
- -grid\_out
- -ijbox
- -import\_text
- -spread
- -text


### Generic Behavior:


 When a file is opened by wgrib2, both the file handle and file name
are saved. This allows wgrib2 supports multiple opens to the same file. For example,


```

  wgrib2 IN.grb -if ":HGT:" -grib OUT.grb \
                -if ":TMP:" -grib OUT.grb \
                -if ":UGRD:" -grib OUT.grb \
                -if ":VGRD:" -grib OUT.grb

```


writes the HGT, TMP, UGRD and VGRD to OUT.grb as you would expect. Technically
the 4 -grib options use the same file handle. Now if you change the above command
so that the last -grib writes to ./OUT.grb instead of OUT.grb. 


```

  wgrib2 IN.grb -if ":HGT:" -grib OUT.grb \
                -if ":TMP:" -grib OUT.grb \
                -if ":UGRD:" -grib OUT.grb \
                -if ":VGRD:" -grib ./OUT.grb

```

You and I know that all 4 -grib options refer to the same file. However, wgrib2 
only does a string check and uses two file handles to refer to the same file.
Confusion occurs and the resultant OUT.grb will be corrupted. Therefore,
you must use consistent naming when referring to the same file.

### Persistance:



By default, wgrib2 saves the file name and file handle for opened files.
This can be handy with callable wgrib2. You can save the time required
for multiple opens of the same file. However, this persistence of the
file name and file handle can require some care. Operating systems
can have a limit on the number of open files, so you may have to close files that
are no longer needed. For some operations, you may want to 
rewind a file.



See also: 
















----

>Description: special files

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/special_file_names.html>_