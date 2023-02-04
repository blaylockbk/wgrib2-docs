
### wgrib2: -ijbox



### Introduction



The -ijbox option takes a rectangular box
of data from a rectanular grid an writes it out in either
text, bin, or spread (sheet) format. The extracted box can
have every point or every n-th point.

### Usage



```

-ijbox i1:i2:di j1:j2:dj output_file format
     format = bin, text or spread   (format of output_file)

     The data in output_file is written out as
      do iy=j1, j2, dj
         do ix=i1, i2, di
            data(ix,iy)
         enddo
      enddo

    Note: -big_endian and -little_endian have no effect the bin type output

```

### Example



 Wgrib2 when writing binary, can make a fortran header at the
beginning of the grid and a fortran trailer at the end of
the grid. I wanted to create a file with fortran header
and trailer surrounding each row of the grid. I was
able to do it with -ijox. The grid of rtgssthr\_grb2
is 1041 x 441.


```

rm output
i=1
while [ $i -le 441 ] ; do
   wgrib2 rtgssthr_grb2 -append -header ijbox 1:1041 $i:$i output bin
   i=`expr $i + 1`
done

```


See also: 
[-undefine](./undefine.html),
[-small\_grib](./small_grib.html),
[-text\_col](./text_col.html),








----

>Description: out   X..Z,A grid values in bounding box X=i1:i2[:di] Y=j1:j2[:dj] Z=file A=[bin|text|spread]

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ijbox.html>_