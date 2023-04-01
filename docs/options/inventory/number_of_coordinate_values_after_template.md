# -number_of_coordinate_values_after_template

## Introduction

Vertical coordinates can be easy like "400 hPa", "2 meters above ground" or
"sigma=0.4". However, the situation is more complicated with the vertical
coordinates in the models. In these model coordinates, the
vertical are usually an integer. You need some extra information to locate
the level in physical space. This information is stored in the PDT (section 4) as the "optional list of coordinate values" at the end of the product
defintion template.

The -number_of_coordinate_values_after_template option
prints the number of values (4 byte) in the "optional list of coordinate values".
The WMO documentation only specifies the hybrid coordinate values should be
in pairs of IEEE single precision floats.

## Usage

```
-number_of_coordinate_values_after_template
   prints section 4, octets 6-7 as an unsigned integer
   it is expected that each value uses 4 octets of storage a the end of Section 4.
```

### Example

```
$  wgrib2 -number\_of\_coordinate\_values\_after\_template COSMO\_EU\_1rec.grib2 -get\_byte 4 6 2
1:0:number_coordinates_values_in_pdt=45:4-6=0,45
```

See also:

```

```

---

> Description: inv

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/number_of_coordinate_values_after_template.html>_
