# wgrib2: -scaling_0001

### Scaling_0001

The standard NCEP post-processor uses data files that specify the
precision of every field (grid values have values of i\*10^j\*2^k,
where j and k are specified by the data file). Different models
may have different precisions even though they use the
same NCEP post-processor.

Wgrib2, by default, writes new fields using the ECMWF-style
which specifies the binary precision, n. (grid values have values
i\*2^j, where the values of i ranges from 0 to 2^(n+1) - 1)

The
-scaling_0001 option converts a field from ECMWF-style
precision to NCEP-style precision. Since everybody has a different scaling,
-scaling_0001 was designed to be an example on how
to write an option that applies your precisions. I am assuming
that are creative enough that there will be no duplicate names. I will
include scaling\_\* to the wgrib2 distributions.

## Usage

```

-scaling_0001

```

### Example

```

wgrib2 IN.grb -scaling_0001 -set_grib_type c3 -grib_out OUT.grb

```

### To add your own Scaling

```

1) cd (wherever)/grib2/wgrib2
2) cp Scaling_0001.c Scaling_mymodelv1.c
          (note code needs to start with a capital)
3) edit Scaling_mymodelv1.c
     don't forget to change scaling_0001 to scaling_mymodelv1 everywhere
4) cd ..
(compile wgrib2 using directions)

See also:



```

---

> Description: misc changes scaling testing (sample)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/scaling_0001.html>_
