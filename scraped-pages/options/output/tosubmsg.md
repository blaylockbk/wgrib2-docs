# wgrib2: -tosubmsg

## Introduction

The -tosubmsg option writes out the current field as a
submessage in a larger grib message. This can save space as various
sections do not have to be repeated. For example, often you only
need one grid definition section for all the submessages. This
feature can save space when the bitmaps are repeated. By combining
the fields into one message, only one copy of the bitmap is needed.
The -tosubmsg option will only write out one grib message.
Fields that cannot be added to the grib message will be ignored with warning.
Fields cannot be added to the the submessage if section 0 (except the message length)
or section 1 differ.

Note that the -tosubmsg is the only grib output option which cannot
write to a pipe. However, the somewhat similar -ncep_uv option
is pipe compatible.

By combining several grib messages into one big message, you can save
space. The practical drawbacks include

- A few codes cannot handle submessages
- Decoders tend to load the entire grib message into memory.
  Loading a 2+ GB grib message would be a problem on 32-bit machines and
  machines with limited memory.
- Inefficient with "partial-http downloading" (of concern to web servers)

However, submessages can be very important for some grids. One grid template stores
the latitude and longitude of each grid point. This is a large overhead per grib
message but by using submessages, the total overhead is reduced.

## Usage

```

-tosubmsg OUTPUT_FILE
NOTE: OUTPUT_FILE cannot be a pipe.

```

### Example

```

$ wgrib2 test.grb2 -tosubmsg new.grb2

1:0:d=2008120200:TMP:800 mb:anl:
2:4786:d=2008120200:TMP:750 mb:anl:
3:9572:d=2008120200:RH:800 mb:anl:
4:13335:d=2008120200:RH:750 mb:anl:
5:17098:d=2008120200:TMP:2743 m above mean sea level:anl:

Submessage statistics:
- # submessages written  : 5
- Kbytes saved           : 0
- Kbytes written         : 20
$ wgrib2 new.grb2
1.1:0:d=2008120200:TMP:800 mb:anl:
1.2:0:d=2008120200:TMP:750 mb:anl:
1.3:0:d=2008120200:RH:800 mb:anl:
1.4:0:d=2008120200:RH:750 mb:anl:
1.5:0:d=2008120200:TMP:2743 m above mean sea level:anl:

```

See also:
[-GRIB](./GRIB.html),
[-ncep_uv](./ncep_uv.html),
[-submsg](./submsg.html)

---

> Description: out X convert GRIB message to submessage and write to file X

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/tosubmsg.html>_
