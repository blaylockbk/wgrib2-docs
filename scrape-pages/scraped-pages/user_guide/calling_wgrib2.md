# wgrib2: Calling wgrib2 from C

## Introduction

There 3 basic ways to call wgrib2 from a C program. The simpler way is to call
wgrib2 and when wgrib2 finishes execution, control is returned to the calling
program. A more complicated way to run wgrib2 in another process and communicate
between the two processes.

### Simple: system call

Calling wgrib2 from C can be as simple as calling the system function

```
    system("wgrib2 IN.grb -match ":TMP:2 m above ground:anl:" -csv tmp2m.csv");
```

This approach is simple but there is overhead of initializing and running wgrib2
as well as opening and closing of the files for every call to wgrib2. All
the communication with wgrib2 is through the file system.

### Wgrib2 as a subroutine, CW2

Wgrib2 v2.0.2 introduced the ability to call wgrib2 as a subroutine (CW2). This avoids
the overhead of loading wgrib2 into memory every time you run wgrib2. Time is
also saved by keeping the files open between calls. Memory files were introduced to
wgrib2, so that the calling program can encode and decode buffers and avoid using
the disk to communicate with wgrib2.

```
    int argc;
    char argv[MAX_ARGC][MAX_STRLEN];
    ...
    ierr = wgrib2(argc, argv);
```

More information is given in [callable_wgrib2](./callable_wgrib2.md). This
facility was used to create a fortran api to read and write grib2.

### Wgrib2 as a Process (Filter)

A program can run wgrib2 as a process and communicate to wgrib2 by
a pipe. In this example, the main program has grib
data and it wants the data regridded or converted into netcdf.

```
#include <stdio.h>
#include <stdlib.h>

/*
 * how to call wgrib2 from a C program.
 *
 * in this example, the main program writes grib data to a pipe,
 * wgrib2 reads from the pipe, regrids the data and saves in a file.
 *
 * This example shows how to call wgrib2 from a C program
 *
 */

#define INPUT_FILE  "../examples/gep19.t00z.pgrb2af180"


int main() {

   FILE  *input, *wgrib2_input;
   int c,err;

   /* open a file with "test" data */
   input = fopen(INPUT_FILE,"rb");
   if (input == NULL) exit(1);

   /* run wgrib2 to read from stdin and interpolate to a new grid */

   wgrib2_input = popen("wgrib2 - -new_grid_winds earth -new_grid ncep grid 221 out.221.grb","w");
   if (wgrib2_input == NULL) exit(2);

   while ( (c = fgetc(input))  != EOF) {
	fputc(c, wgrib2_input);
   }

   /* close the wgrib2 program */
   err=pclose(wgrib2_input);
   printf("all done, err=%d\n",err);

   return 0;
}
```

A grib encoder was developed using wgrib2 as a process. The main program would write
data to a pipe and wgrib2 would read the pipe and write a grib file. The advantage
of this approach is the the writing is multitasking. The disadvantage is that
it was difficult to develop a grib reader using this approach. Eventually CW2 was
developed to solve the grib reader problem and CW2 became the appropriate approach
for writing grib.

|

|     |
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
[Climate Prediction Center Web Team](/comment-form.md)
Page last modified: Oct 25, 2016
| [Disclaimer](https://weather.gov/disclaimer.php) | [Privacy Policy](https://weather.gov/privacy.php) |

|

---

> Description: calling wgrib2 from C

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/calling_wgrib2.html>_
