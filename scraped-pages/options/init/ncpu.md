# wgrib2: -ncpu

### The number of threads

Wgrib2 can be compiled to use multiple threads. By
default, an OpenMP-enabled version of wgrib2 will use the environement
variable OMP_NUM_THREADS to control the number of threads.
If the environment variable OMP_NUM_THREADS is not
defined, the number of threads created is usually the same as the number
of cores. The -ncpu option controls the number of threads
that will be used by wgrib2. If wgrib2 is not compiled to use OpenMP, this
option is quietly ignored. The -ncpu option overrides
the OMP_NUM_THREADS environment variable.

When you run an OpenMP-enabled wgrib2 version on a shared system, the
performance may be suboptimal if you use too many threads. For example, on one of
our linux systems, we have 32 cores. The system is shared by many users and
during a busy time, there may only be a few cores free at any one time. When
using 32 threads, there is often a thread that doesn't get much time in the beginning
and is slow to finish.

The main usage of the -ncpu option is to allocate
threads/cpus when you are running multiple copies of wgrib2.

## Usage

```

-ncpu N
  N = number of threads

```

### Example

```

   $ wgrib2 IN.grb -ncpu 3 -new_grid_winds grid -new_grid ncep grid 221 - | wgrib2 - -ncpu 1 -set_grib_type j -ncep_uv OUT.grb

```

The above line uses 3 threads for regridding and one thread for jpeg2000 compression. The jpeg2000
compression routines can't take advantage of more than one thread.

See also:

---

> Description: init X number of threads, default is environment variable OMP_NUM_THREADS/number of cpus

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ncpu.html>_
