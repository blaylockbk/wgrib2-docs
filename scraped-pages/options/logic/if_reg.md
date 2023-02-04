# wgrib2: -if_reg

## Introduction

The reverse polish notation calculator (-rpn) allows you
to manipulate the grid values and save the results in registers. The values in
the registers are persistent until they are cleared or the program (wgrib2) ends.
The persistent registers allow you to do calculations that require multiple
fields. For example, to calculate the wind speed, you have to save the zonal
wind in a register and the meridional wind in another register. When both
registers have values (U and V), you can then proceed with the wind speed
calculation.
The -if_reg option tests to see if specified registers have
been set. If the register have been set, then the options up to and including the
next output option are executed like with an -if option.

Here is an example of computing the 500 mb wind speed.

```

$ wgrib2 a.grb -match ":[UV]grd:500 mb:anl:" \
 -if ":UGRD:" -rpn "sto\_1" -fi \
 -if ":VGRD:" -rpn "sto\_2" -fi \
 -if\_reg 1:2 \
 -rpn "rcl\_1:sq:rcl\_2:sq:+:sqrt:clr\_1:clr\_2" \
 -set\_var WIND \
 -grib\_out out.grb

     line 1: only process the U and V at 500 mb
     line 2: store U 500mb analysis in register 1
     line 3: store V 500mb analysis in register 2
     line 4: if (register 1 and register 2 have values then
     line 5: calculate the wind speed: sqrt(reg_1**2 + reg_2**2)
     line 6: set variable time to WIND (wind speed)
     line 7: write out the WIND data to a grib file
             -grib_out is an output option and ends the -if block

     Note: this is a very simple script and that doesn't check the matching
     date code, grid type, etc.

```

With operational NCEP files, the V field immediately follows the corresponding U field.
If we assume that this is always true, then the following computes all the wind speeds.

```

$ wgrib2 a.grb -match ":[UV]grd:" \
 -if ":UGRD:" -rpn "sto\_1" -fi \
 -if ":VGRD:" -rpn "sto\_2" -fi \
 -if\_reg 1:2 \
 -rpn "rcl\_1:sq:rcl\_2:sq:+:sqrt:clr\_1:clr\_2" \
 -set\_var WIND \
 -grib\_out out.grb

```

## Usage

```

-if_reg X

X is a list of register names, ex. 1:2 or 2:4:7

```

See also:
[-if](./if.md),
[-fi](./fi.md).
[-rpn](./rpn.md),

---

> Description: if X if rpn registers defined, X = A, A:B, A:B:C, etc A = register number

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/if_reg.html>_
