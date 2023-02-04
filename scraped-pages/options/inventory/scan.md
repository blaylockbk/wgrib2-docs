# wgrib2: -scan

## Introduction:

A grib message contains the grid point values. If you are a C programmer, the grid would
naturally be in "C" order. A fortran programmer would have the grid in fortran order.
Some people would start from the NW corner and others would start from the SW corner.
In grib1, all these scan orders are possible. The grib1 scan orders are,

1. WE:SN

- WE:NS
- EW:SN
- EW:NS
- SN:WE
- NS:WE
- SN:EW
- NS:EW

You read the above notation (WE:SN) by first go from W to E then go from S to N.

```

AB:CD
   do i = C to D
     do j = A to B
       data
     enddo
   enddo

```

Grib2, includes the above 8 scan orders and adds 4 more useful
scan orders.

1. WE|EW:SN

- WE|EW:NS
- EW|WE:SN
- EW|WE:NS

```

AB|BA:CD
   odd_row=true
   do i = C to D
     if (odd_row) {
       do j = A to B
         data
       enddo
     }
     else {
       do j = B to A
         data
       enddo
     }
     odd_row = not odd_row
   enddo

```

This "plow the field" order reduces the file size when using
regional grids and saving the increments.

Grib2 has 16 scan orders of which 3 are common: WE:SN, WE:NS and WE|EW:SN.
Wgrib2 simplfies life by internally converting fields to WE:SN by default.

## Usage

```

-scan

```

### Example

```

-$ wgrib2 ds\_ens.grb -scan
1:0:scan=5 input=WE|EW:SN output=WE:SN
2:218709:scan=5 input=WE|EW:SN output=WE:SN
3:434276:scan=5 input=WE|EW:SN output=WE:SN
...
input=WE|EW:SN   the grib file is in WE|EW:SN scan order
output=WE:SN     the internal registers and output files except for grib
                 will be in WE:SN order, use the -order option to change
                 the output scan order

```

See also:
[-order](./order.md),

---

> Description: inv scan order of grid

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/scan.html>_
