# wgrib2: -wind_speed

## Introduction

The -wind_speed option takes the zonal and meridional winds,
computes the wind speed and writes it out in grib format. For this option to
work, the meridional wind must follow the corresponding zonal wind for that
level, time and forecast hour. If the U and V in your grib file do not have
this order, the file must be sorted. However, many NCEP forecast files already
have this order.

You can save computer time by using the -match option
to restricting the decoding of records to U and V.

## Usage

```

-wind_speed output_grib_file

   Comment: you can write the output of -wind_speed and -wind_dir to the same file

```

### Example

```

$ wgrib2 gep19.t00z.pgrb2af180 -wind\_dir wind.grb -wind\_speed wind.grb -match "(UGRD|VGRD)"
4.1:86046:d=2009060500:UGRD:200 mb:180 hour fcst:ENS=+19
4.2:86046:d=2009060500:VGRD:200 mb:180 hour fcst:ENS=+19
8.1:226831:d=2009060500:UGRD:250 mb:180 hour fcst:ENS=+19
8.2:226831:d=2009060500:VGRD:250 mb:180 hour fcst:ENS=+19

$ wgrib2 wind.grb
1:0:d=2009060500:WDIR:200 mb:180 hour fcst:ENS=+19
2:97922:d=2009060500:WIND:200 mb:180 hour fcst:ENS=+19
3:179554:d=2009060500:WDIR:250 mb:180 hour fcst:ENS=+19
4:277476:d=2009060500:WIND:250 mb:180 hour fcst:ENS=+19

```

See also: [-match](./match.md),
[-wind_dir](./wind_dir.md)
[-wind_uv](./wind_uv.md)

---

> Description: out X calculate wind speed, X = output gribfile (U then V in datafile)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/wind_speed.html>_
