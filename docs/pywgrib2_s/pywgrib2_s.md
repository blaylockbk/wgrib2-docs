### pywgrib2_s: Home Page

## Introduction

Pywgrib2_s is a python package for reading and writing grib2 which is a World
Meterological Organization (WMO) format for exchanging meteorological and oceanographic
gridded data. The specifications for the grib2 (grib version 2) format are available from
the WMO, and several libraries are available for reading and writing grib2. Pywgrib2_s
is based on the fortran wgrib2api and the wgrib2 library.

The design goals of pywgrib2_s are a simple interface, minimal requirements for
additional python modules/packages, and the ability to program for efficient I/O.
Since pywgrib2_s is uses the wgrib2 library, search terms are based on wgrib2 terms.
For example, searching for the 2 meter temperature is done by, var="TMP", lev="2 m above ground".

8/2020: I am using the alpha pywgrib2_s in my work, with no problems. The problems
that I am using it are a bit too complicated for a wgrib2 script, and not so resource
intensive that I that I need fortran.

### Basics: Grib Data Model

The grib data model is more complicated than the data model for the natural world.
For example, the earth's temperature is defined by a location and time (x,y,z,t). Grib was developed
for forecast centers has more characteristics such as forecast time, averaging period,
ensemble member, and numerical model that created the forecast. When you consider
tracer gases, you add chemical type. Aerosols add the composition and size of the pariculate matter.
Forecasts can be deterministic or probabalistic.

Grib files often have multiple vertical coordinate systems. Meteorologists often
use pressure or isentropic coordinates. The aviation community often use height above a
constant geopotential. The people on the ground often want meters above ground.
The ocean fields and soil fields will have their own vertical coordinates.

A grib file does not include an index, usually a software package includes
a mechanism for making an index file. Pywgrib2_s uses the human-readable wgrib2
inventory. Not having a included index has its advantages and disavantages.
Having a human-readable index has been very advantageous.

A grib field has many characteristics to uniquely identify it. So pywgrib2_s has
taken the approach that characteristics of the field will be text string which
grow in time as the grib standard adds more characteristics. To find a field,
you provide search terms that compared with the inventory.

### Naming Convention

Grib is an international standard which doesn't specify a naming standard. For
example a pressure surface of 1000 Pascals, could be called "10 mb" (non-SI unit),
"10 hPa" (used in scientific literature), "1 kPa" (SI unit) or even 0.2953 inch of nercury.
At NCEP, geopotential height has the name HGT. This is a local convention.
Pywgrib2_s, like wgrib2, use the NCEP [NCEP naming convention](https://www.nco.ncep.noaa.gov/pmb/docs/grib2/grib2_doc/). Grib allows locally defined tables, and pywgrib_s/wgrib2 wgrib2
follows the recommendation of the defining site.

### Reading and Writing Grib

```
 [mk\_inv(grib, inv\_file)](./pywgrib2_s_mk_inv.md)                   makes inventory file
 [read\_inv(inv\_file)](./pywgrib2_s_read_inv.md)                       reads inventory file, returns list
 [inq(grib,...)](./pywgrib2_s_inq.md)                            inquire about grib2 file
                                            read metadata, grid point data,
                                            calculate lat-lon of grid points for common grids
 [write(grib,...)](./pywgrib2_s_write.md)                          write grib2
 [close(file)](./pywgrib2_s_close.md)                              close file, used to flush, free up resources,
```

### Memory Files

The above routines read and write files. Using the the filesystem would be a bottleneck for many
applications, so wgrib2 added memory files. These memory files resided in the memory space
of wgrib2 and can be accessed from python.

```
 memory file:  @mem:N   N = 0,1,..,29   memory files 10-29 are reserved for use by pywgrib2_s

 get_bytes_mem(fileno)                    returns bytes (buffer) from from memory file number "fileno"
 get_dbl_mem(fileno)                      reserved for future use
 get_flt_mem(fileno)                      returns numpy.ndarray (numpy array) with float32 values
 get_str_mem(fileno)                      returns text string from memory file number "fileno"
 mem_size(fileno)                         returns size of memory file number "fileno" in bytes
 set_mem(fileno,buffer)                   copy buffer to memory file number "fileno"
                                          buffer can be type bytes, str or numpy.ndarray
                                          set_mem(12,'') will empty @mem:12
```

### Multi-Threading

Pywgrib2_s uses the multi-threaded wgrib2 library to speed up execution. You control the number
of threads by setting the environment variable OMP_NUM_THREADS. You rarely set the number
of threads greater than 5 because the speedup becomes minor in most cases.

There are a few wgrib2 operations that can use as many cores as possible
(ex. -ensemble processing, -lola for certain grids). For calls to wgrib2(..),
the number of threads can be controlled by -ncpu option.

### Global Variables

Pywgrib2_s uses global variables for the

1. [configuration](./pywgrib2_s_global_variables.md)- [output of pywgrib2_s.inq(..)](./pywgrib2_s_global_variables.md)

### Low level Access: wgrib2

The pywgrib2_s is a python module that calls wgrib2. Sometimes you may
need to know the version and configuration of the wgrib2 library.
The pywgrib2_s.wgrib2(..) will let you directly call wgrib2.

```
 wgrib2_version()                           returns a string with the version of the
                                            wgrib2 library. Same as wgrib2 -version

 wgrib2_config()                            returns list of strings with the configuration
                                            of the wgrib2 library. Same as wgrib2 -config

 [wgrib2( [ (list of wgrib2 arguments) ] )](./pywgrib2_s_wgrib2.md)   call wgrib2


```

### Low Level Access: Registers

Registers are part of the reverse polish notation calculator (-rpn). They are used by the internals
of pywgrib2_s, and should only be used by programs that make direct calls to wgrib2.

```
 reg_size(regno)                            returns size of register number "regno" in elements
 reg_shape(regno)                           reserved for future use
 get_flt_reg(regno)                         returns numpy.ndarray (numpy array) with floating point 32 value
 get_dbl_reg(regno)                         reserved for future use
 set_reg(regno, array)                      copy array (numpy.ndarray) to RPN register "regno"
```

### Requirements:

The requirements for pywgrib2_s are

1. python 3.5+, numpy

- Linux (tested: Ubuntu, Redhat, SUSE), MacOS, Windows (tested: Cygwin-64)
- shared wgrib2 library created using wgrib2 v3.0.2
- Linux compilers: gcc/gfortran, AOCC clang/flang, NVidia HPD sdk nvc/nvfortran
- MacOS compilers: homebrew gcc/gfortran (gcc may be a wrapper for clang)
- Windows compilers: cygwin gcc/gfortran

### Cookbook

A living cookbook of pywgrib2_s "recipes" is being maintained.
The cookbook serves as a learning tool for beginners, a simple
validation tool, testbed for testing the utility of the API, and
and starting point for writing some code.

Li Xu of CPC has provided some documentation for the
[cookbook](https://www.cpc.ncep.noaa.gov/products/people/lxu/cookbook/) and pywgrib2_s routines.

### Installation:

See [Installation directions](./pywgrib2_s_install.md)

### Code Timeline

- wgrib2: released 2004
- wgrib2 can be built as subroutine that can be called from C or Fortran, wgrib2 library (2015)
- alpha version of pywgrib2 using wgrib2 library by George Trojan (2/2020)
- wgrib2 v3.0.0 with alpha-beta pywgrib2_s (9/2020) and pywgrib2_xr afterwards
- wgrib2 v3.0.2 with updates for pywgrib2\_\* including Windows support (3/2021)

See also:
[pywgrib2_s conventions](./pywgrib2_s_conventions.md)

---

> Description: Overview

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pywgrib2_s.html>_
