# wgrib2: -set_metadata

## Introduction

Whenever -set_metadata option is "executed", one
line of the data file is read, the metadata is then applied to the current
(sub-)message. For example, your grib file has 3 messages and you
want to alter the metadata. Here is a metadata file
that could be used to alter the grib file.

> 0:0:d=2009010100:HGT:500 mb:anl:scale=0,0:
>
> 0:0:d=2009010100:TMP:2 m above ground:anl
>
> 0:0:d=2009010106:HGT:500 mb:12 hour forecast:scale=0,0

The format of the metadata file resembles the wgrib2 inventory by design.
The first and second fields are ignored by by -set_metadata.
In practice, the first first is the record number and the second field
is the byte location of the template to be used. (A template file
could be a collection of templates.) The third through sixth fields
are the date code, variable name, level and ftime. Finally the remaining
fields are optional and order independent.

The date field has been extended from the default d=YYYYMMDDHH. The date
field can be d=format-N or D=format-N

```

                         Format 1

    YYYY, YYYYMM, YYYYMMDD, YYYYMMDDHH, YYYYMMDDHHmm or YYYYMMDDHHmmss
    where YYYY = 4 digit year
            MM = 2 digit month
            DD = 2 digit day of month
            mm = 2 digit minute
            ss = 2 digit second
    If MM, DD, mm, ss are missing, the values are unchanged.

                         Format 2

  +N(units)  N is an integer, units = hr, dy, mo or yr
             this adds N units to the reference time
             ex. +6hr, +1dy

                         Format 3

  -N(units)  N is an integer, units = hr, dy, mo or yr
             this subtracts N units to the reference time
             ex. -6hr, -1dy

```

```

		Optional fields

    scale=I,J			set decimal scaling (I) and binary scaling (J) for encoding
    encode i*2^J*10^I           set decimal scaling (I) and binary scaling (J) for encoding
                                   same as scale=I,J  v2.0.6+
    encode I bits               store grid values using I bits, no decimal scaling (ECMWF style) v2.0.6+
    grib_max_bits=I             set maximum number of bits used to store grid point data (max 25) v2.0.6+
    packing=S                   set compression/packing to S
                                S=simple,complex1,complex2,complex3,jpeg,aec (long form)
                                S=s,c1,c2,c3,j (short form)
    N%d level                   set percentile
    prob ...                    set probability
    XYZ                         XYZ=ens mean, wt ens mean, ens std dev, cluster std dev, normalized ens std dev,
                                normalizedd cluster std dev, ens spread, ens large anom index, wt ens mean,
                                unwt custer mean, 25%-75% range, min all members, max all members
    ENS=...                     set ensemble member info
                                ENS=hi-res, low-res, +N, or -N
    N ens members               set number of ensemble members
                                ex. 22 ens members
    code table X.Y=Z            set code table, equivalent -set table_X.Y Z   requires wgrib2 v2.0.5+
    flag table X.Y=Z            set flag table, equivalent -set table_X.Y Z   requires wgrib2 v2.0.5+
                                note: not all X.Y have been implemented

             Following options follows -set VAR VAL and do not follow the wgrib2 inventory format
    disciple=
    local_table=
    master_table=
    center=                     set center
    subcenter=                  set sub-center
    background_process_id=
    analysis_or_forecast_process_id=
    table_M.N=                  only for selected M.N

```

To change the metadata, you can do

```

wgrib2 in.grb -s >meta
(change meta)
wgrib2 in.grb -set_metadata  meta -grib out.grb

Note: that -grib does not change the grid point data/packing.

```

The -set_metadata option is used for creating
grib2 files. Note that both
-set_metadata and the various
-import options will change the output
precision. Consequently the
-import option should preceed the
-set_metadatda option.

Note: only a subset of levels and ftime parameters is currently implemented.
The -set_metadatda option
does not support all metadata.

To format of the levels and ftime are
the same was used by wgrib2 inventories.

See also:
[-set_ave](./set_ave.md),
[-set_date](./set_date.md),
[-set_ftime](./set_ftime.md),
[-set_lev](./set_lev.md),
[-set_metadata_str](./set_metadata_str.md),
[-set_scale](./set_scale.md),
[set_var](./set_var.md),

---

> Description: misc X read meta-data for grib writing from file X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_metadata.html>_
