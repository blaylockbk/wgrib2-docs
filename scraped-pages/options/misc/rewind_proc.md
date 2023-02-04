
### wgrib2: -rewind\_proc CW2



### Introduction



The wgrib2 library can either close or not close the files after
calls to the wgrib2 subroutine. Closing the file frees up resources
and helps prevent you from getting the "Too Many Files Open" error
message. However, reopening files is a slow operation and you
usually keep the files open if you plan to read/write to the file again.


 If you are reading from a previously opened and not-closed file,
you will normally continue reading from where the last call to
the wgrib2 subroutine stopped. Sometimes this the preferred behavior.
Sometimes, you want the library to start reading the file from the
beginning.

 The -rewind\_proc option rewinds an
already open file during the processing phase. 

### Usage




```

-rewind_proc FILE
  FILE must be already opened
  This command is only useful in callable wgrib2

```


See also:
[-persistent](./persistent.html)
[-rewind\_final](./rewind_final.html)
[-rewind\_init](./rewind_init.html)
[-transient](./transient.html)










----

>Description: misc  X      rewinds file X on processing step if already opened, CW2

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/rewind_proc.html>_