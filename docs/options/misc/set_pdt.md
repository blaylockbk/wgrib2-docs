
### wgrib2: -set\_pdt



### Introduction



Section 4 of a grib message 
contains the product definition and the product is
defined through the Product Definition Template (PDT). 
There are many different PDTs but only some are in common usage. The
-set\_pdt option changes the current PDT to
another. For example, you want to add ensemble information to a
forecast with no ensemble information. To do this, you have
to change the template to one that has ensemble information
and then fill in the various parts of the PDT.
Another use of the -set\_pdt option is 
when you have a PDT that is unsupported by a program such
as GrADS. You can use this option to 
convert an unsupported PDT to a supported PDT.
To retain metadata, prefix the PDT with a plus sign. The amount of 
metadata copied depends on the version of wgrib2.

### Usage




```

-set_pdt X      X=product defintion template number (example 8, not 4.8), default size, PDT is cleared
-set_pdt +X     X=product defintion template number (example 8, not 4.8), copy metadata from current PDT
                  size may vary depending on current PDT
-set_pdt X:Y    X=product defintion template number (example 8, not 4.8), Y=byte size of PDT if variable-sized PDT
                  PDT is cleared
-set_pdt +X:Y   X=product defintion template number (example 8, not 4.8), Y=byte size of PDT if variable-sized PDT
                  copy metadata
                Note: using the wrong value of Y can produce errors

```

### Example



```

wgrib2 prior to v2.0.2
$ wgrib2 p.grb -set\_pdt 0 -grib OUT.grb
1:0:d=2010111618:var discipline=0 center=7 local_table=0 parmcat=255 parm=255:no_level:-1 missing fcst:
$ wgrib2 p.grb -set\_pdt +0 -grib OUT.grb
1:0:d=2010111618:PRMSL:no_level:-1 missing fcst:

wgrib2 v2.0.2
$ wgrib2 png.grb -grib OUT.grb
1:4:d=2009060500:RH:2 m above ground:330 hour fcst:std dev
$ wgrib2 png.grb -set\_pdt 0 -grib OUT.grb
1:4:d=2009060500:var discipline=0 center=7 local_table=1 parmcat=255 parm=255:no_level:-1 missing fcst:
$ wgrib2 png.grb -set\_pdt +0 -grib OUT.grb
1:4:d=2009060500:RH:2 m above ground:330 hour fcst:

```


Suppose a program has problems with pdt 60 and 61. Changing the pdt to
1 and 11 will solve the incompatibility with only a little loss of metadata.

```

$ wgrib2 IN.grb -if ":pdt=60:" -set\_pdt +1 -fi \
 -if ":pdt=61:" -set\_pdt +11 -fi \
 -grib OUT.grb
  requires wgrib2 v2.0.2+

```



See also: 
[-fi](fi.html),
[-grib](grib.html),
[-if](if.html),
[-Sec4](Sec4.html),
[-pdt](pdt.html),








----

>Description: misc  X      makes new pdt, X=(+)PDT_number or X=(+)PDT_number:size of PDT in octets, +=copy metadata

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_pdt.html>_