# -aerosol_size

## Introduction

The -aerosol_size option prints the
size of the aerosol particle if applicable. This applies
to Product Definition Template 4.44 - 4.48. This option is
part of the standard inventory, -s.

## Usage

```
-aerosol_size
```

### Example

```
$ wgrib2 ngac.t00z.a3df03 -for 2:6
2:77259:d=2014081000:TMP:1 hybrid level:3 hour fcst:
3:131943:d=2014081000:RH:1 hybrid level:3 hour fcst:
4:205847:d=2014081000:MASSMR:1 hybrid level:3 hour fcst:aerosol=Dust Dry:aerosol_size >=2e-07,<2e-06:
5:243914:d=2014081000:MASSMR:1 hybrid level:3 hour fcst:aerosol=Dust Dry:aerosol_size >=2e-06,<3.6e-06:
6:279272:d=2014081000:MASSMR:1 hybrid level:3 hour fcst:aerosol=Dust Dry:aerosol_size >=3.6e-06,<6e-06:

Message 4: mass mixing ratio (MASSMR) of Dry Dust, particle size ranges from 2e-7 meters to 2e-6 meters

$ wgrib2 ngac.t00z.a3df03 -for 2:6 -aerosol\_size
2:77259:
3:131943:
4:205847:aerosol_size <=2e-07,&gt2e-06
5:243914:aerosol_size <=2e-06,>3.6e-06
6:279272:aerosol_size <=3.6e-06,>6e-06
```

See also:
[-aerosol_wavelength](aerosol_wavelength.md),

---

> Description: inv optical properties of an aerosol

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/aerosol_size.html>_
