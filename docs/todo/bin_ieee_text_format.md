# wgrib2: bin, ieee, text formats

### The bin, text and ieee formats

The -bin, -text, -ieee
and -lola options create bin/text/ieee format files. The
format doesn't change from that used by wgrib. The difference between bin and ieee
is that bin writes the numbers in the machine's native format and ieee writes
the numbers in big_endian ieee format.

```

			Text Format, with header


(grid 1)	(nx) (ny)		grid dimensions
		(grid value)
		(grid value)
		...
		(grid value)		nx*ny values
(grid 2)	(nx) (ny)		grid dimensions
		(grid value)
		(grid value)
		...
		(grid value)		nx*ny values
(grid 3)	...



		        Text Format, with no header

(grid 1)	(grid value)
		(grid value)
		...
		(grid value)		nx*ny values
(grid 2)	(grid value)
		(grid value)
		...
		(grid value)		nx*ny values
(grid 3)	...


	This format is not a recommended format because your code
could easily read too many or few grid points.



			Binary and IEEE, no header

(grid 1)	(binary float value)		grid value
		(binary float value)
		...
		(binary float value)		nx*ny values

(grid 2)	(binary float value)
		(binary float value)
		...
		(binary float value)		nx*ny values
		etc

	The no-header format is not a recommended format to use.
The format does not indicate the size of the grid. If your program
makes a mistake on the grid dimension, you could get interesting results.
However, some fortran compilers require this format for binary files (Cray,
ABSoft under AmigaOS).  In addition, this is the default GrADs binary format.



			Binary and IEEE, with header


(grid 1)	(binary integer)	nx*ny*sizeof(float)
		(binary float value)
		...
		(binary float value)	nx*ny values
		(binary integer)	nx*ny*sizeof(float)

(grid 2)	(binary integer)	nx*ny*sizeof(float)
		(binary float value)
		...
		(binary float value)	nx*ny values
		(binary integer)	nx*ny*sizeof(float)


    The binary-with-header format is commonly used by UNIX
fortrans for their binary files.  Some MS-DOS fortran compilers
also support this format.  You can use this format with GrADS
with the "options sequential" line in the control file.

```

Note, the order of the numbers is controled by the
-order option.

See also: [-text](./to_do/text.md),
[-bin](./to_do/bin.md),
[-ieee](./to_do/ieee.md),
[-lola](./to_do/lola.md),
[-order](./to_do/order.md),

---

> Description: bin, ieee, text formats

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/bin_ieee_text_format.html>_
