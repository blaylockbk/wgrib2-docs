### Submessages vs Messages in wgrib2

In grib-2 format was designed for transmission of meteorologial data. So
it breaks up the information into messages. If you have a message, you
have the complete information about the field or fields. In theory,
one could save a space by having hundred of fields in one message.
However, the space savings are minor vs the cost of needing much
free memory to decode the file. So modern convention is
to put one field per message (no submessages) unless you have
multiple field need to be grouped together. For example, U and V
can be placed in the same message because you need both to know
the winds at any level. The use of submessages has become less common
because the space saving (maybe 100 bytes per field) is minor when
the models started outputing high resolution grids.

### How does wgrib2 handle submessages

When you want an inventory of the file, you want to see all the
fields. If you want to extract the 500 hPa heights, you doesn't care whether it is
in a submessage. The only difference when using wgrib2 is that record number
may have decimal point and the submessage number if needed.
Most of the time, one can igore whether the data are in submessages.

In this wgrib2 inventory, the data are in one message with four submessages.

> 1.1:0:d=2000070312:HGT:500 mb:24 hour fcst
>
> 1.2:0:d=2000070312:TMP:850 mb:24 hour fcst
>
> 1.3:0:d=2000070312:UGRD:850 mb:24 hour fcst
>
> 1.4:0:d=2000070312:VGRD:850 mb:24 hour fcst

In this wgrib2 inventory, the data are in four messages.

> 1:0:d=2000070312:HGT:500 mb:24 hour fcst
>
> 2:114221:d=2000070312:TMP:850 mb:24 hour fcst
>
> 3:195862:d=2000070312:UGRD:850 mb:24 hour fcst
>
> 4:277503:d=2000070312:VGRD:850 mb:24 hour fcst

For either file, we can do

> wgrib2 grib2_file -match ":HGT:500 mb:" -text z500.txt
>
> or
>
> wgrib2 grib2_file | grep ":HGT:500 mb:" | wgrib2 grib2_file -i -text z500.txt

Wgrib2 "ignores" the submessage feature and makes the
the file look and handle like a file without submessages.
However, there are two options that allow you to preserve
the "submessage" character of the original grib file. The
option "-GRIB" writes out the entire grib message including
all submessages. The companion option "-submsg N", only
allows inventory processing of submessage N. (Usually N is set to one.)

### Message mode

Wgrib2 has a few options that work with messages.

```
-GRIB [file]              .. write grib message that was just read to [file]
-if_n I:J:K               .. if message number is in do-loop I:J:K
-match "^[0-9]*\.1:"      .. only process first submessage
-match "^[0-9]*(:|\.1:)"  .. only process grib messages or first submessage
-ncep_uv                  .. combine ugrd, vgrd into one message
-submsg_uv                .. combine vectors into one message
-tosubmsg                 .. combine into one message
```

---

> Description: Submessages vs messages in wgrib2

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/submessages.html>_
