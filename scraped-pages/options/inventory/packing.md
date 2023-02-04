
### wgrib2: -packing



### Introduction



The grib format is for storing gridded data. Usually gridded data is
stored as an scaled integer that has been packed or compressed. 
Exceptions are spectral data and tabular data (data are entries to a
table).




The -packing option shows how the gridded
data was packed. The most common packing methods are jpeg2000,
one of the various complex packing schemes. If you increase
the verbosity by -v -packing, you 
see the packing method, scaling factors and range of possible
integers.



### Usage




```

-packing

```

### Example



```

$ wgrib2 png.grb2 -packing
bash-4.1$ wgrib2 png.grb2 -packing
1:4:packing=grid point data - png compression,_
$ wgrib2 png.grb2 -v -packing
bash-4.1$ wgrib2 png.grb2 -v -packing
1:4:packing=grid point data - png compression,_ val=(0+i*2^0)*10^-1, i=0..65535 (#bits=16)

The file, png.grb2, was packed using the obsolete png compression. The
grid point values can have the values (0+i*2^0)*10^-1 where i is an integer 
that ranges from 0..65535.

$ wgrib2 small.grb2 -packing
1:0:packing=grid point data - simple packing,s
$ wgrib2 small.grb2 -v -packing
1:0:packing=grid point data - simple packing,s val=(1.22666e+06+i*2^2)*10^-2, i=0..4095 (#bits=12)

The file small.grb2 is using simple packing, integers are stored using 
12 bits (#bits=12). The grid points can have values of (1.22666e+06+i*2^2)*10^-2 
where i is an integer that ranges from 0..4095.

```



See also: 





----

>Description: inv          shows the packing mode (use -v for more details)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/packing.html>_