# wgrib2: -persistent

## Introduction

When wgrib2 opens a file, the file may be marked as persistent or
transient. If a file is marked as transient, wgrib2 will close
the file as part of its normal "finalize" procedure. If a file
is marked as persistent, the file will closed when the system
terminates the program. If you run the wgrib2 utility, it makes
no difference who closes the files. However, when you use the
C/Fortran API, it makes a major difference. When you write
C/Fortran programs that calls the wgrib2 API, you may want to
use the same file thousands of times. It is better to open
the file once rather than thousands of times.
The -persistent option marks an already
open file as persistent.
The -transient option marks an already
open file as transient. When opening a file, the default
is to mark the file as persistent.

## Usage

```

-persistent FILE
  FILE must be already opened

```

See also:
[-transient](./transient.html)

---

> Description: init X makes file X persistent if already opened (default on open), CW2

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/persistent.html>_
