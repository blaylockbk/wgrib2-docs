# -stats, -max, -min

## Introduction

The -unix_time option writes unix time in the inventory.
Unix time is defined by the number of seconds after 00:00 UTC Januay 1, 1970.
Unix time is used by the many operating systems, and by NetCDF files.
The -unix_time option depends on system/library functions
to calculate the unix time. It requires ansi C compatibility and it is
subject to overflows depnding on the definition of time_t.

The unix time is stored in time_t integer-type variable.
If time_t is a signed 32-bit integer, the integer will overflow in 1/19/2038
and is only valid between 12/13/1901 and 1/19/2038.
One valid solution to the year 2038 overflow problem is to define time_t as an unsigned 32-bit integer.
Then valid unix time are from 1/1/1970 to 2/7/2106. This solves the year 2038 overflow problem
but makes time prior to 1970 invalid.
The most common solution
is to make time_t a signed 64-bit integer which limits the largest year to 2,147,485,547.
That's enough time for dinosaurs to come and go. Here are the list
of systems which may have problems with unix_time.

- wgrib2 prior to v3.1.1, unix_time is converted to int (typically 32-bit int)
- 32-bit linux systems, time_t is 32-bit signed int until kernel 5.6, API needs to be fixed
- 32-bit QNX: 32-bit ONX use unsigned 32-bit integer for time_t
- old versions of BSD varients use signed 32-bit int for time_t
- Windows: depends on the compiler

## Usage

```
-unix_time
   unix_time is the number of seconds after 00:00 UTC 1/1/1970
   prints the unix time for the reference and verification times

   v3.1.1+: if the code detects a problem with the reference time, a fatal error occurs
   prior to v3.1.1: if the code detects a problem with the time, -1 is printed
      if there is no forecast time (ex. radar), the verification time is -1
```

### Example

```
$ wgrib2 test.grb2 -unix\_time
1:0:unix_rt=1228176000:unix_vt=1228176000

unix_rt is the reference time
unix_vt is the verification time
```

See also:
[-t (reference time)](./t.md),
[-vt (verification time)](./vt.md),

---

> Description: inv print unix timestamp for rt & vt

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/unix_time.html>_
