
### pywgrib2: close(file)



### Introduction



Pywgrib\_s opens a file when you use it, and automatically closes
the file when the program ends. You may want to manually close file
because

1. the system limits the number of open files, and you 
 need to close some files before reaching the system limit.
 - You need to flush the file buffes so a program other than pywgrib2\_s
 can read the file.
 - pywgrib2\_s had an error and the file state needs to be cleaned.
 - free up memory used to buffer the reads or writes to a no longer used file


### Example



```

>>> pywgrib2_s.mk_inv('a.grb','a.inv',Short=True)
0
>>> pywgrib2_s.close('a.grb')
0
>>> pywgrib2_s.close('not-a-file')
1

```

### Usage



```

     a=pywgrib2_s.close(file)
         if file is a memory file, the memory file is set to zero length
         if file is open, it closes the file, frees the resources, and returns 0
                closing the file will flush any write buffers
         if file not open, it returns 1

```


[overview](./pywgrib2_s.html)
[back](./pywgrib2_s_write.html)






----

>Description: close file, used to flush, free up resources

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pywgrib2_s_close.html>_