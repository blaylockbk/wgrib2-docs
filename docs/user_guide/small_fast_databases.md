
### How to make small fast grib2 databases



### Introduction



The old joke is that an engineer tells his client that he
can design a gizmo that is fast, cheap and reliable, and
the client gets to choose two. The same principle applies
to many projects including making a grib-2 database. Here,
the desired qualities are small file size, speed for the users
and effort required to set up the database. For small databases,
you probably want to minimize the set-up effort. For larger
databases, all three factors come into play. Here are some
hints for minimizing size/speed-of-use. They all require some
set-up effort.

### Making a smaller database


1. Have unneeded fields?  

Use wgrib2 to remove records  
- Smaller grids take less space  

Use wgrib2 or some other program to make regional subsets  
- Data stored at higher than needed precision?  

Use wgrib2 to reduce the precision  
- Better compression available?  

Use wgrib2 or cnvgrib  
- use submessages  

Use wgrib2


### Faster to read


1. Jpeg2000 compression is makes small files but very slow to read.  

I am finding that complex packing with spatial differencing is a   

good trade off between speed and size. If the field has undefined values   

then complex packing is smaller than Jpeg2000 and faster to read.  
- Decode routines that have been parallelized are faster. Wgrib2 has   

parallel packing and complex packing without bitmaps routines.  
- Smaller grids are faster  
- Reorder the records to maximize sequential access  

Use sort and wgrib2. This is important for optical media.  
- Use an index/inventory.  
- Too many submessages can use up available memory.


### Minimizing Effort in Setting up the Database


1. Save grib data as provided  
- Writing grib encoding software?  

 IEEE Grib-2 is the easiest format to write. It's produces
 big files and is not compatible with some grib-2 decoders.
  

 You can use wgrib2 to convert from ieee grib2 to a more common packing.


### Effects of Compression on Sample Files



```


45x91 grid, 5 fields

         simple complex1 complex2 complex3    jpeg
size      21884    16030    13623    13917   11514  bytes
read      0.001    0.001    0.001    0.001   0.012  seconds user time
write     0.002    0.006    0.006    0.006   0.017  seconds user time

note: write time includes a "simple" read operation
note: the creating of the complex packing is heuristic
and different algorithms will produce different results.

4320x2160 high resolution SST field

         simple complex1 complex2 complex3    jpeg
size      11054    4076      2365     2280    3252  K bytes
read       0.38    0.41      0.42     0.42    2.66  seconds user time
write      0.78    2.06      1.76     1.68    4.20  seconds user time

note: write time includes a "simple" read operation
note: the creating of the complex packing is heuristic
and different algorithms will produce different results.

The commands to change the packing:

wgrib2 in.grb -set_grib_type X -grib_out out.grb
X=simple, complex1, complex2, complex3, jpeg


```



| 

|  |
| --- |
| 

---

 |
| [NOAA/](https://www.noaa.gov/)
[National Weather Service](https://www.nws.noaa.gov/)
[National Centers for Environmental Prediction](https://www.ncep.noaa.gov/)
 Climate Prediction Center
 5830 University Research Court
 College Park, Maryland 20740
[Climate Prediction Center Web Team](/comment-form.html)
 Page last modified: Aug 7, 2009
  | [Disclaimer](https://weather.gov/disclaimer.php) |  [Privacy Policy](https://weather.gov/privacy.php) |

 |





----

>Description: How to make small fast grib2 databases

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/small_fast_databases.html>_