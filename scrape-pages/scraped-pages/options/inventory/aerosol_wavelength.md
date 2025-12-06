# wgrib2: -aerosol_wavelength

## Introduction

The option, -aerosol_wavelength, prints the
optical properties of the aerosol particle. This option is part
of the standard inventory, -s.

## Usage

```
-aerosol_wavelength
```

### Example

```
$ wgrib2 wrib2 ngac.t00z.a2df03 -aerosol\_wavelength -d 1
1:0:aerosol_wavelength <=5.45e-07,>=5.65e-07
```

See also:
[-aerosol_size](aerosol_size.md),

---

> Description: inv optical properties of an aerosol

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/aerosol_wavelength.html>_
