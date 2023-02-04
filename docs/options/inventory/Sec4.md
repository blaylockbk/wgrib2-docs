# wgrib2: -Sec4

## Introduction:

The -Sec4 option prints a short summary of Section 4, the
product definition section.

```

$ wgrib2 -Sec4 png.grb2
1:4:Sec4 len=36 #vert coordinate=0 Product Defn Template=4.2 size=36 free=0

len=36                        Section 4 is 36 octets/bytes in length
#vert coordinate=0            no vertical coordinates have been defined
Product Defn Template=4.2     using Product Definition Template 4.2  (Code Table 4.0)
size=36                       Size of PDT excluding vertical coordinates
free=0                        Should be zero, len-size-8*#vert_coordinates

```

## Usage

```

-Sec4

```

See also:
[-pdt](pdt.html),

---

> Description: inv Sec 4 values (Product definition section)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/Sec4.html>_
