
### wgrib2: -AAIGlong



### Introduction



The -AAIGlong option is similar to
the -AAIG option except for a new file name convention for the output.
The -AAIGlong option 
writes the data into a Ascii ArcInfo Grid file. This option 
is experimental and only supports equally spaced lat-lon
grids. (You can use the -new\_grid option of wgrib2 to create
an equally spaced lat-lon grid.) Each field is written to different file in the current directory.

### File name convention for ouput: \*.asc



The file name convention 

```

   NAME=(wgrib2 -S)
   remove (message number)[.submessage number]:(byte location): 
   remove trailing semicolon if any
   replace "/" by " DIV "
   replace "\" by " BS "
   replace ":" by "_"
   replace "'" by " Q "
   replace '"' by " Q "

   Note: The (wgrib2 -S) output can change between different versions of wgrib2.
   The grib table can be updated.
   The new metadata can be added to the inventory in order to uniquely 
     identify fields.
   In rare cases, the format of the metadata has been updated.

   If you need to format of the NAME to be unchanging, please freeze the
    version of wgrib2.

```

### Usage




```

-AAIGlong

```

### Example




```

$ wgrib2 gep19.t00z.pgrb2af180 -match "HGT:500 mb" -AAIGlong
raster file: D=20090605000000_HGT_500 mb_180 hour fcst_ENS=+19.asc
9:280952:d=2009060500:HGT:500 mb:180 hour fcst:ENS=+19
$ ls \*asc
D=20090605000000_HGT_500 mb_180 hour fcst_ENS=+19.asc

```

The above line converts all the 400 mb HGT fields into an
arcinfo ascii grid file.



See also: [-AAIG](./AAIG.html)
[-new\_grid](./new_grid.html)












----

>Description: out          writes Ascii ArcInfo Grid file, lat-lon grid only long-name *.asc (alpha)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/AAIGlong.html>_