# -names

## Introduction (alpha)

WMO has defined how fields like temperature
are encoded into grib. For example, temperature has to have
(discipline, category, and parameter) to be (0, 0, 0). However, WMO never
defined a name for like "t" or "TMP" for the temperature.
Of course saying "discipline=0, category=0, and parameter=0"
is unwieldy, so the centers have adopted names for the various
fields.

The -names option changes the names of
the WMO defined fields to either the DWD, ECMWF or NCEP names or convention.
The grib format allows for locally defined fields. Centers can make extensions
to the grib table which apply to their file from that center. These fields
will use the names as defined by the originating center. The default naming
convention being set at compile time.

The -names
option is in an alpha stage of development. Our use of the ECMWF and DWD grib table
needs more work. The ECMWF and DWD names often mix the variable
(discipline, category, parameter) with other orthogonal parameters
such as

1. level, ex 2 meters above ground

- statistical processing (in time), ex average over last 24 hours
- grid, latitude on F grid
- satellite information (instrument type)
- aerosol type
- MRMS (NSSL Mult-Radar/Multi-Sensor) adds probability to the grib name

For wgrib2 v3.0.1, if the ECMWF or DWD grib name depends on any of the above parameters, the
name is not included in the wgrib2's grib table. This causes some problem because some of
the ECMWF grib names don't apply when the orthogonal parameter is not defined.
As I said before, this approach is in alpha testing.

The geometrical height has the names "dist" and "h" in the ECMWF table. Wgrib2 v3.0.1-v3.0.3
is using "dist". Using "dist" versus "h" may change in
later versions of wgrib2, depending on which is more commonly used. (Feedback please)

The use of the ECMWF and DWD grib has not been finalized. So use -names at your own risk.

Wgrib2 has options that are dependent on the grib names such as
vector interpolation in -new_grid. At present, there is no automatic
conversion of the names.

## Usage

```
-names CENTER
       CENTER  =  ncep (default in the public-release makefile)
                  ecmwf
                  dwd

       This option changes the names of the WMO defined fields to either the NCEP, DWD or
       ECMWF conventions.  The locally defined fields are not affected.

The default convention is a compile-time option (USE_NAMES in the makefile).
To see the default, run "wgrib2 -config" and look for "default WMO names:".
```

### Example

```
 wgrib2 wind.grb -names ecmwf
1:0:d=2009060500:wdir:200 mb:180 hour fcst:ENS=+19
2:97922:d=2009060500:ws:200 mb:180 hour fcst:ENS=+19
3:179554:d=2009060500:wdir:250 mb:180 hour fcst:ENS=+19
..
 wgrib2 wind.grb -names ncep
1:0:d=2009060500:WDIR:200 mb:180 hour fcst:ENS=+19
2:97922:d=2009060500:WIND:200 mb:180 hour fcst:ENS=+19
3:179554:d=2009060500:WDIR:250 mb:180 hour fcst:ENS=+19
```

### NCEP Names

The NCEP grib names are unique, a name refers to a unique quantity.
However, the same grib name may be defined both locally and by the WMO.
Since WMO approval of a new field may take several months, NCEP usually makes
a local definition in order to speed up development.
NCEP considers duplicate names to be a mistake (same name but different quantities).
When the wgrib2 sets the variable name (ex. -set_var, -set_metadata), the WMO definition
is used if local and WMO definition exists.

### DWD Names

The DWD grib names are unique except for a few cases (wgrib2 v3.0.1)
which were probably an oversights. There are WMO defined fields
which do not have a DWD name.

Some DWD names are not orthogonal because the names include information about
the level or timing. They are not included in the wgrib2 tables.

### ECMWF Names

The ECMWF grib names are not unique. Names may refer to
different quantities. For example, uv could have units of
m^2/s^2 or m/s^2. For decoding grib, the duplicate
names do not pose a problem. However, using
the -set_var or -set_metadata option will have problems
with names that are duplicated. Wgrib2 will use the
first definition that it finds.

Some ECMWF names are not orthogonal, they include information about
the level or timing. The some non-orthogonal variable names
are not included in the wgrib2 tables. For example, "2t"
which is the temperature two meter above the ground (or water surface).
The "2t" grib is defined as the air temperature with a level of 2 meter
above the ground. So "2t" can be thought as "t" with a level of "2 meters
above the ground". The wgrib2 script that reads the ECMWF parameter database
determines that "2t" is a non-orthogonal variable because it needs to have
a level of "2 meters above ground". As result, "2t" is not wgrib2's gribtable.

Some non-orthogonal variable names are included in the wgrib2 tables. For example,
"10spg10" is defined as "10 metre wind speed probability of at least 10 m/s". The
ECMWF database indicates this parameter doesn't depend on the level or any probability
or level information. Since this variable is not conditional on the level or
probability templates, this variable is added to wgrib2's tables.

See also:

---

> Description: init X grib name convention, X=DWD, dwd, ECMWF, ecmwf, NCEP, ncep

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/names.html>_
