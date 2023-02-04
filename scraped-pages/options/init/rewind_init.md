
### wgrib2: -rewind\_init CW2



### Introduction



The wgrib2 library can either close or not close the files after
calls to the wgrib2 subroutine. Closing the file frees up resources
and helps prevent you from getting the "Too Many Files Open" error
message. However, reopening files is a slow operation and you
usually keep the files open if you plan to read/write to the file again.


 If you are reading from a previously opened and not-closed file,
you will normally continue reading from where the last call to
the wgrib2 subroutine stopped. Sometimes this is the preferred behavior.
Sometimes, you want the library to start reading the file from the
beginning.

 The -rewind\_init option rewinds an
already open file in the setup or pre-processing phase. It
is commonly used to rewind the inventory when searching from matching
records.

### Usage




```

-rewind_init FILE
  FILE must be already opened
  This command is only useful in callable wgrib2

```


See also:
[-persistent](./persistent.html)
[-rewind\_final](./rewind_final.html)
[-rewind\_proc](./rewind_proc.html)
[-transient](./transient.html)










----

>Description: init  X      rewinds file X on initialization if already opened, CW2

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/rewind_init.html>_