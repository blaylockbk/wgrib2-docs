# wgrib2: -wind_uv

## Introduction

The -wind_uv option takes the wind speed and wind
direction and computes the zonal (UGRD) and meridional (VGRD) wind
components. The wind components are then written out in grib format.
For this option to work, the corresponding wind direction and speed
must not be separated by a non-corresponding wind speed or direction.
If the speed and direction do no have this order, the file must be sorted.

If the wind direction is earth (grid) relative, the UGRD and VGRD
winds are earth (grid) relative.

You can save computer time by using the -match option
to restricting the decoding of records to wind speed (WIND) and
wind direction (WDIR).

## Usage

```

-wind_uv output_grib_file

```

### Example

```

$ wgrib2 gep19.t00z.pgrb2af180 -wind\_dir wind.grb -wind\_speed wind.grb -match "(UGRD|VGRD)"
4.1:86046:d=2009060500:UGRD:200 mb:180 hour fcst:ENS=+19
4.2:86046:d=2009060500:VGRD:200 mb:180 hour fcst:ENS=+19
8.1:226831:d=2009060500:UGRD:250 mb:180 hour fcst:ENS=+19
8.2:226831:d=2009060500:VGRD:250 mb:180 hour fcst:ENS=+19

$ wgrib2 wind.grb -wind\_uv uv.grb
1:0:d=2009060500:WDIR:200 mb:180 hour fcst:ENS=+19
2:97922:d=2009060500:WIND:200 mb:180 hour fcst:ENS=+19
3:179554:d=2009060500:WDIR:250 mb:180 hour fcst:ENS=+19
4:277476:d=2009060500:WIND:250 mb:180 hour fcst:ENS=+19

$ wgrib2 uv.grb
1:0:d=2009060500:UGRD:200 mb:180 hour fcst:ENS=+19
2:89777:d=2009060500:UGRD:200 mb:180 hour fcst:ENS=+19
3:179554:d=2009060500:UGRD:250 mb:180 hour fcst:ENS=+19
4:269331:d=2009060500:UGRD:250 mb:180 hour fcst:ENS=+19


```

See also: [-match](./match.md),
[-wind_speed](./wind_speed.md)
[-wind_dir](./wind_dir.md)

---

> Description: out X calculate UGRD/VGRD from speed/dir, X = output gribfile

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/wind_uv.html>_
