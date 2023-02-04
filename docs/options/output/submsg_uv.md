# wgrib2: -submsg_uv

## Introduction

The option -submsg_uv is like the option
-ncep_uv. The latter option puts UGRD and VGRD into
the same grib message. The former combines all the vector fields into
their own grib message.
The option -submsg_uv may be important for
parallel processing. For example, you want to regrid from a Gaussian
grid to a polar stereographic grid that covers the North Pole.
For scalar fields, the grid can be regridded independently which
allows a possibility of parallel processing. Vector fields have
to processed together (U and V). One approach is to put the
vector fields into their own grib message (combine the corresponding
U/V fields). Then all the grib messages can be regridded independently.

The vector fields are the same as used by the
-new_grid option. The vector list can
be changed using the -new_grid_vectors option.

## Usage

```

-submsg_uv output_file

```

### Example

```

$ wgrib2 test.grb2 -match ":500 mb:" -submsg\_uv output.grb

```

See also:
[-tosubmsg](./tosubmsg.md)
[-submsg](./submsg.md)
[-submsg_uv](./submsg_uv.md)
[-ncep_uv](./ncep_uv.md)
[-new_grid_vectors](./new_grid_vectors.md)

---

> Description: out X combine vector fields into one message

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/submsg_uv.html>_
