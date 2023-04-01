# -ens_qc

## Introduction (wgrib2 v3.1.1)

The option -ens_qc tests the
ensemble members looking for extreme values, values that
are extreme relative the spread of the ensemble members.
This test is done for each grid point. An unreasonable
scaled extreme value provides a simple quality control
for an egregiously bad ensemble member.

During the production of the Conventional Observations REanalsyis (CORe),
a glitch was found where the upper-level ozone was two orders of magnitude
larger than expected. The problem was caused by a single ensemble
member which had an irreproducible problem which showed up in a single layer
of a tracer. The -ens_qc option was written to
detect extreme values of the ensemble members for quality control.
In addition, -ens_qc
can be used to calculate the ensemble mean and spread faster
than -ens_processing.

The option -ens_qc will QC ensemble
members which limits this option to PDTs of 0, 1, 8, and 11.
PDTs of 1 and 11 are explicity ensemble members. PDTs of 0 and
8 are often used by the control runs. The code should be updated
to include other ensemble PDTs.

The contents of the output files will vary depending the qc_version.
As of 1/2022, only qc_version 1 has been defined.

```
1) ens_mean,  sum(x(i))/n,  i=1..n where n is the number of ensemble members
2) ens_spread, sqrt(sum((x(i)-em)**2)/n)  note: n is used rather than n-1
3) ens_min = minimum value over all ensemble members (for each grid point)
4) ens_max = maximum value over all ensemble members (for each grid point)
5) scaled extreme value, max((ens_max-ens_mean),(ens_mean-ens_min))/ens_spread
6) grid maximum of the scaled extreme value (single number)

(1)-(4) are written to arg1, order: (3), (4), (1), (2)
(5) is written to arg2
(6) is written to arg3

Note: the calculations are done grid point by grid point. If all the
members have UNDEFINED values for a particular grid point, then variables
1-5 are set to UNDEFINED for that grid point.  If the ensemble spread
is zero, then the scaled extreme value is set to zero.  The calculation
differs from -ens_processing which requires no missing values for all
the ensemble members before calculating the various products. This
will affect the calculation of the mean cloud top temperature some
ensemble members are missing clouds.
```

The -ens_qc is unlike most wgrib2
options in that this option can use large amounts of memory. Suppose
that you have an 80 member ensemble and are processing the tmp500 field.
This option stores all 80 tmp500 fields in memory. As the size of the grid and the number
of ensemble member increases, the required memory will increase.

### Code Table 4.7

Grib Code Table 4.7 is used to specify whether the field
is the ensemble mean, spread, min, max or scaled extreme value.
The ensemble mean, spread, min and max are WMO defined values.
For the scaled extreme value, wgrib2 is using the locally defined NCEP
"extreme forecast index". Since the NCEP documentation doesn't define
the index, wgrib2 will be using the scaled extreme value. So
other uses of the "extreme forecast index" will differ.

The scaled extreme values are stored in 8 bits precision (two digits),
and can only be written if the original file is from NCEP because
there is no equivalent WMO code for extreme value (1/2022).

### Definition of an Ensemble Member

The -ens_qc option uses the same defintion of ensemble member as
-ens_processing. Any grib message which is
not identified as an ensemble member is ignored by -ens_qc.
So you cannot use -ens_qc on the ensemble-mean.
The wgrib2 code [ed. written 3/2022 v3.1.1] that identifies ensemble member
is unreasonably simplistic. Ensemble members are identified by having a PDT
of 0, 1, 8 and 11. As a result, aerosols, chemical tracers, simulated satellite data,
and post-processing of ensemble members are ignored.

### Order of Fields

The -ens_qc requires fields to be in a specific order,
the same order as in -ens_processing. Please
see the documentation for -ens_processing.

## Usage

```
-ens_qc FILE1 FILE2 FILE3 QC_VERSION
   FILE1 = ensemble min, ensemble max, ensemble mean, ensemble spread
           output in grib2 format
   FILE2 = if center == NCEP, (ensemble) scaled extreme value
           output in grib2 format
           if center != NCEP, no output is written
   FILE3 = grid maximum of the (ensemble) scaled extreme value
           text, single line per field
   QC_VERSION = the type of QC to be run
           1  only acceptable value (1/2021)
```

### Example

```
$ gmerge - sfg_2014060500_fhr06_mem0* | wgrib2 - -ens_qc out1 out2 out3 1
1:0:d=2014060412:UGRD:0-1 hybrid pressure layer:12 hour fcst:ENS=+1
2:183334:d=2014060412:UGRD:0-1 hybrid pressure layer:12 hour fcst:ENS=+2
3:366283:d=2014060412:UGRD:0-1 hybrid pressure layer:12 hour fcst:ENS=+3
4:549841:d=2014060412:UGRD:0-1 hybrid pressure layer:12 hour fcst:ENS=+4
5:733631:d=2014060412:UGRD:0-1 hybrid pressure layer:12 hour fcst:ENS=+5
...
  gmerge - (list of ensemble members)
           The output file is "-" which is a convention for stdin/stdout depending
             on the expectation. Since we expecting an output file, the "-" is stdout.
           Takes the 1st grib message from each ensemble member and writes it to stdout.
           Takes the 2nd grib message from each ensemble member and writes it to stdout.
           etc
           The gmerge step puts the data into proper order assuming,
              the grib file contains no sub-messages,
              the indivdual grib files are in the same order.
           gmerge is not Windows compatilble.

  wgrib2 - -ens_qc out1 out2 out3 1
           The input file is "-" which is a convention for stdin/stdout depending
             on the expectation. Since we expecting an input file, the "-" is stdin.
           writes min, max, mean and spread to out1
           writes scaled extreme values to out2
           writes grid maximum scale extreme value to out3

  Note: piping grib data to stdout and from stdin should not work in Windows.
  I have seen it work, and I have seen it fail. My standard practice is to
  avoid pipes in Windows.

$ wgrib2 out1
1:0:d=2014060412:UGRD:0-1 hybrid pressure layer:12 hour fcst:min all members
2:262325:d=2014060412:UGRD:0-1 hybrid pressure layer:12 hour fcst:max all members
3:524650:d=2014060412:UGRD:0-1 hybrid pressure layer:12 hour fcst:ens mean
4:786975:d=2014060412:UGRD:0-1 hybrid pressure layer:12 hour fcst:ens spread
5:1000148:d=2014060412:UGRD:1-2 hybrid pressure layer:12 hour fcst:min all members
...

$ wgrib2 out2
1:0:d=2014060412:UGRD:0-1 hybrid pressure layer:12 hour fcst:extreme forecast index
2:131253:d=2014060412:UGRD:1-2 hybrid pressure layer:12 hour fcst:extreme forecast index
3:262506:d=2014060412:UGRD:2-3 hybrid pressure layer:12 hour fcst:extreme forecast index
...

$ cat out3
UGRD:0-1 hybrid pressure layer:max scaled extreme=7.454012
UGRD:1-2 hybrid pressure layer:max scaled extreme=8.029185
UGRD:2-3 hybrid pressure layer:max scaled extreme=7.936052
...
```

### GrADS

At the time of writing (1/2021), the files that
are produced by
-ens_qc cannot be displayed
using the g2ctl/gribmap/GrADS set of programs. However, they
can be displayed by atl_g2ctl/alt_gmp/GrADS set of programs.

```
  alt_g2ctl -short output >output.ctl
  alt_gmp output.ctl
  grads
```

See also:
[-ens_processing](./ens_processing.md),

---

> Description: out X..Z,A simple qc ensemble members X=stats.grb Y=extreme.grb Z=extreme.txt A=1 (qc_version)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ens_qc.html>_
