# wgrib2: a sample function

## Introduction

Wgrib2 is "function" driven. Options on the command line corresponds to a function call.
When wgrib2 is started, each option-function is initialized by calling it with mode=-1.
For each grib record processed, the option-function is called with mode=verbosity index (verbosity ≥ 0).
After all grib records have been processed, the option-functions are told to clean up by calling
them with mode=-2.
Here is a sample function.

> #include <stdio.h>
>
> #include "grb2.h"
>
> #include "wgrib2.h"
>
> #include "fnlist.h"
>
> extern int decode;
>
> extern int need_output_file;
>
> /\*
>
> \* HEADER:100:min:inv:0:print minimum value
>
> \* the above line is needed by each command line option
>
> \* HEADER:sort-order:type-of-function:number-of-arguments:description
>
> \*/
>
> int f_min(int mode, unsigned char \*\*sec, float \*data, int ndata, char \*inv_out, void \*\*local) {
>
> double mn;
>
> int ok, i;
>
> if (mode == -1) decode = 1; // decode=1 so wgrib2 will unpack grib data
>
> else {
>
> mn = ok = 0;
>
> for (i = 0; i < ndata; i++) {
>
> if (!UNDEFINED_VAL(data[i])) {
>
> if (ok) mn = mn < data[i] ? mn : data[i];
>
> else { ok = 1; mn = data[i]; }
>
> }
>
> }
>
> if (ok) sprintf(inv_out, "min=%lg",mn);
>
> else sprintf(inv_out, "min=undefined");
>
> }
>
> return 0;
>
> }
>
> > ```
> >
> > The arguments to f_min() are the standard for options that do not require an argument
> >   mode = -1: initialization  -2: cleanup  O+: processing verbosity index
> >   sec = sections of the grib (sub) message
> >   *data = decoded grid point values
> >   ndata = number of grid points
> >   *inv_out = string that written to the inventory
> >   **local = unique pointer, used for "static" variables.
> >
> > ```
> >
> > |
> >
> > |     |
> > | --- |
> >
> > |
> >
> > ---
> >
> > |
> > | [NOAA/](https://www.noaa.gov/) > > [National Weather Service](https://www.nws.noaa.gov/) > > [National Centers for Environmental Prediction](https://www.ncep.noaa.gov/)
> > Climate Prediction Center
> > 5830 University Research Court
> > College Park, Maryland 20740
> > [Climate Prediction Center Web Team](/comment-form.html)
> > Page last modified: July 27, 2016
> > | [Disclaimer](https://weather.gov/disclaimer.php) | [Privacy Policy](https://weather.gov/privacy.php) |
> >
> > |

---

> Description: writing a simple function

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/function.html>_
