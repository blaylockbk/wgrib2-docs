# wgrib2: -ave_var

## Introduction

The -ave_var option is based on
the -ave option. In addition to
computing the mean, the -ave_var option
computes the sample standard deviation, minimum and maximum
values for each grid point. Welford's method for computing
the mean and variance was used because it is a
one-pass scheme with the accuracy of a two-pass algorithm.

The options and expected sequence of fields is the same as
with the -ave option.

## Usage

```
-ave_var (time interval)  (output grib file)
   time interval = (integer)(units)
   (units) = hr, dy, mo, yr
```

### Example

```
$ wgrib2 pgblnl.gdas.201302 -match ":UGRD:200 mb:" -ave\_var 6hr out
141:1170314:d=2013020100:UGRD:200 mb:anl:
659:5936420:d=2013020106:UGRD:200 mb:anl:
1177:10703131:d=2013020112:UGRD:200 mb:anl:
1695:15432277:d=2013020118:UGRD:200 mb:anl:
...
$ wgrib2 out
1:0:d=2013020100:UGRD:200 mb:112@6 hour ave(anl),missing=0:
2:13343:d=2013020100:UGRD:200 mb:112@6 hour StdDev(anl),missing=0:
3:24058:d=2013020100:UGRD:200 mb:112@6 hour min(anl),missing=0:
4:37401:d=2013020100:UGRD:200 mb:112@6 hour max(anl),missing=0:
```

Making a GrADS control for "out" requires editing the ctl file to shorten
the names of variables.

```
$ alt\_g2ctl -0t out > out.ctl
$ vi out.ctl
.. edit out.ctl so the last 6 lines look like this
vars 4
UGRDdev 0 0 "UGRD:112@6 hour StdDev(anl),missing=0:200 mb" * 200 mb U stddev
UGRDave 0 0 "UGRD:112@6 hour ave(anl),missing=0:200 mb" * 200 mb U ave
UGRDmax 0 0 "UGRD:112@6 hour max(anl),missing=0:200 mb" * 200 mb U max
UGRDmin 0 0 "UGRD:112@6 hour min(anl),missing=0:200 mb" * 200 mb U min
endvars
$ alt\_gmp out.ctl
wgrib2_flags=-npts -set_ext_name 1 -T -ext_name -ftime -lev
wgrib2_inv=.invd02
dtype:  dtype grib2
tdef:  nt=1 start=00Z01feb2013 by=1mo
zdef: nlevel=1
resolve_dsets dset=out inctime=1mo
resolve_dsets: no template
scanning out (process=0)
merge index files
writing out index
/export/cpc-lw-webisuzak/wd51we/bin/alt_gmp v0.0.5 finished ctl=out.ctl records matched=4, not matched=0 ctl_defn=4
```

See also:
[-ave](./ave.md)

[https://jonisalonen.com/2013/deriving-welfords-method-for-computing-variance/](https://jonisalonen.com/2013/deriving-welfords-method-for-computing-variance)

---

> Description: out X Y average/std dev/min/max X=time step, Y=output

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/ave_var.html>_
