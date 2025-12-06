# Selecting messages

> TODO: This could use some work. 
> - Don't focus on wgrib command.
> - Link to relevant commands.
> - Clarify what "submessages" are

Grib files contain many grib messages (i.e., records or "fields"). Often, you only want to process selected fields.

With the older wgrib software you could process all `-d all`, specific message number `-d N`, or by using `grep` and the `-i` option.

```bash
wgrib -d all grb                         # writes all records to binary file to "dump"
wgrib -d 10  grb                         # write record 10 to binary file "dump"
wgrib grb | grep ":HGT:" | wgrib -i grb  # write HGT fields to "dump"
```

Wgrib2 is very similar. The differences is that `-d all` is not needed and writing a binary file is not a default option.

```bash
wgrib2 -bin dump grb                                   # write all records to binary file "dump"
wgrib2 -d 10   -bin dump grb                           # write record 10 to binary file "dump"
wgrib2 -d 10.1 -bin dump grb                           # write record 10.1 to binary file "dump"
wgrib2 grb | grep ":HGT:" | wgrib2 -i grb -bin dump    # write HGT fields to "dump"
wgrib2 -match ":HGT:" grb -bin dump                    # new syntax: write HGT fields to "dump"
```

One difference is record numbers may have a "decimal point". With grib2, for example, the U and V winds may be in the same grib2 record. In the wgrib2 notation, the first _submessage_ would be N.1 (or N) and the second _submessage_ would be N.2.

---

> Description: selecting messages

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/selecting_messages.html>_
