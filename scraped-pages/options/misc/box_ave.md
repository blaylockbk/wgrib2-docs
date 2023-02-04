# wgrib2: -box_ave

## Introduction

The -box_ave option does a spatial smoothing
by doing a simple box average of the data field. Amount of smoothing
can be controlled by the size of the box.
The -box_ave option can be used on regional
and global fields. To identify global fields, you can use
the option -cyclic.

## Usage

```

-box_ave DX DY CRITICAL_WEIGHT
   DX=width of box (in grid points), DX has to be an odd positive integer
   DY=height of box (in grid points), DY has to be an odd positive integer

   The box average is the mean value for a box of DX x DY centered on
   the grid point.

   CRITICAL_WEIGHT
      -1: grid(i,j) = UNDEFINED    if original grid(i,j) is undefined
                    = box average  if original grid(i,j) is defined
       not -1: let wt = number of grid points that are defined in the box
            grid(i,j) = UNDEFINED     if wt <= WT
                      = box average   if wt > WT

The speed of -box_ave is O(NX*NY*DY). The O(NX*NY) method was slower
because of poor cache utilization and false sharing.

```

### Example

I had a high-resolution Gaussian grid and wanted to convert it to a 1x1 degree grid.
There were about 81 grid points in a 1 degree cell. The budget interpolation
in -new_grid worked but it was slow and worked by taking 25 bilinear interpolations
and averaging them to make the budget interpolation. So the pre-existing solution
was slow and slighly inaccurate. To interpolate scalars to the 1x1 grid, you can
run a box_average with 9x9 grid and then use -new_grid to get the cell average values.
This method is, as expected, slightly smoother than the budget interpolation of
-new_grid. For vectors, you have to use the budget interpolation of the -new_grid option.

See also: [-new_grid](./new_grid.md),

---

> Description: misc X Y Z box average X=odd integer (lon) Y=odd integer (lat) critical_weight

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/box_ave.html>_
