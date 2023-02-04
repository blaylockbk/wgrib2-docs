### Options

## Introduction

The wgrib2 command line consists of a list of options and a grib file
to be processed. The options like
-if,
-else, and
-endif can alter the 'run_flag' which is the basis
of the IF-ELSE-ENDIF blocks.

### Types of Options

```

wgrib2:      *.c:
------       ---
if           If                For IF/ELSE/ELSEIF/ENDIF type options
else         Else
elseif       Elseif
endif        Endif

init         setup             only called in the beginning, for set up

inv          inv               print output to stdout (make an inventory)
inv>         inv_output        print to a file (make an inventory)
out          output            write to a file with a non-inventory output, ex binary
misc         misc              any thing else

```

### How "Options" are called

### What is "mode"

Each option is associated with a subroutine. Each option is "called"
for setup (mode == -1). This allows
the option to do initialization such as setting up the flags,
checking the arguments and opening files. This is the only
time that init or setup options are called.
The -match and -d options are init options because
it sets up the grib processor to only accept certain grib messages.

The options are then called for every grib message that is processed.
In the grib-processing phase, mode ≥ 0 and the mode will indicate the
verbosity level. Mode == 0 is the lowest verbosity. and mode == 2
is normally the highest verbosity. These verbosity modes can be set by -v,
-v1 and -v2. Modes 97, 98 and 99 are used in debugging.

When the program has finished processing the grib data, the
option subroutines are called to cleanup the processing
(mode == -2). The
cleanup step is used to finish calculations (ex. averaging),
close files and free memory. Freeing memory and closing files
may seem a wasted effort because the operating system will do that
when a program finishes. However, wgrib2 is also a subroutine so
memory recovery and closing files is a must otherwise you
may run out of memory or file handles.
By the way, if you don't want a file closed, you can mark the file as persistant.

### Custom Options

Writing your own option can be easy. That is why there
are so many options in wgrib2 (387 in 7/2022). You just have
to follow some rules.

### Setup: mode = -1

In the setup phase, you have to request services. For
example if you want the grid point values or locations,
you set at flag in this step.
You can also parse the arguments in the setup phase
or grib-processing step. If you process the arguments
in setup phase will save time. Processing the arguments
in the grib-processing step is necessary if the
processing depends on the grib message. In addition,
macro options can only call options that have
no setup. Here is an example of setting the options.

```

extern decode, latlon, save_translation;
..
    if (mode == -1) {
        decode = 1;             /* decode the grid point values */
        latlon = 1;             /* calculate the lat lon values */
        save_tranlation = 1;    /* save the translation from external to internal scan order */
        return 0;
    }

```

### Setup: flags

The following flags request a service. Set the flag to
one if you want the service. Do not set the flag to zero because
another option may want the service.

```

extern int decode             grid point values are decoded
extern int latlon             latitude and longitudes are calculated
extern int save_translation   save trnsalation from grib file to internal scan order

```

Some flags are set by other options, and all options are expected
to obey the flag. These flags may change.

```

file_append                    Open an file for writing in append mode
ieee_little_endian             if writing ieee, use little endian
header                         files use write in "header" mode
flush_mode                     flush the output after writes (all output files)

```

### Grib Processing: mode ≥ 0

In this phase, the option routine is called one
time each grib (sub)message is processed. The
routine has potential access (if requested) to the grid
point data and locations. Of course, the routine
is not called if prevented by if/else/endif block.

### Grib Processing: mode = -2

In the cleanup phase, calcuations, I/O are finished,
files are closed and allocated memory

### FILE I/O

Wgrib2 allows multiple options to read and write to a file.

$ wgrib2 IN.grb -if ':TMP:' -bin OUT.bin -endif -if ':HGT:' -bin OUT.bin -endif

If each -bin option were allowed to open the file, the writes would
not work correctly. So opening, closing, writing, reading and fseeks
have to be done by a special set of routines.

```

    fopen_file(..)
    fclose_file(..)
    fread_file(..)
    fwrite_file(..)
    fseek_file(..)
    ftell_file(..)
    fflush_file(..)

    the only difference from the normal I/O is FILE *file is replaced by struct seq_file *file

```

### Static Variables

An option gets called for initialization, grib-processing, and cleanup. So
the option routine needs static variables to retain information such
as the open file handles. The situation is more difficult because an
option can be used more than once on a command line.

```

 $ wgrib2 IN.grb -if ':TMP:' -bin TMP.bin -endif -if ':HGT:' -bin HGT.bin -endif

```

So the option routine needs a set static variables for each use of the
option on the command line.

```

struct static_vars {
    struct seq_file out;
    int num_run;
}
...
    struct  static_vars *save;
    if (mode == -1) {
        decode = 1;
        *local = save = (struct static_vars *) malloc( sizeof(struct static_vars));
        /* local is defined as the subroutine argument, the i-th option on the command line
           is passed local[i] */
        if (save == NULL) fatal_error("memory allocation in XYZ);
        save->num_run = 0;
        if (fopen_file(&(save->out), arg1, "wb") != 0) fatal_error("open file %s", arg1);
        return 0;
    }
    save = *local;	/* *local is passed from calling routine */
    if (mode == -2) {
        fprintf(stderr,"XYZ called %d times\n", save->num);
        fclose(&(save->out));
        free(*local);
    }
    if (mode >= 0) {
        fwrite_file(data, ndata, sizeof(float), &(save->out));
        save->num_run += 1;
    }
    return 0;

```

### Stdout

Normally options do not write to stdout but write their output to a buffer.
This allows the option routines to be called by other routines and return
a string value. For example,
you wanted the name of the variable. You can call the
the -var option, and the name would be returned
in the buffer.

### Adding a New Option

Adding a new option involves updating tables with
(1) option name and corresponding subroutine name,
(2) number of arguments the option takes and the
(3) description of the option for the help command.
On a POSIX machine (has a POSIX shell and utilities),
the process is process is automatic. You need to do,

1. The name of the source code needs to start with a capital letter
   and be written in C.

- Source code file needs to be added to grib2/wgrib2/ ex. grib2/wgrib2/File.c
- Each option needs a HEADER line
  - ex. \* HEADER:100:text:output:1:write text data into X
  - The line must start with '\* HEADER:'
  - then a 3 digit number which display priority
  - then the name of the option, ex. 'text'
  - then comes the type of option, ex. 'output'
  - then by the number of arguments (only 0..8 are allowed)
  - finished by a description of the option- The name of the routine associated with the option is int f_OPTION(ARGN)
- OPTION is replaced by the option name in the header line
- N is replaced by the number of arguments in the header line
- The script function.sh will read the header lines and generate fnlist.h and fnlist.c

---

> Description: options

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/types.html>_
