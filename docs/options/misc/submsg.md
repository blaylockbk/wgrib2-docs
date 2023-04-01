# -submsg

## Introduction

Usually wgrib2 ignores the distinction between messages and submessages; wgrib2 treats
sugmessages like messages except that the "message number" in the inventory is replaced
by a "message number.submessage number" in the inventory. If you want to work with
submessages, the following options are availble.

1. -GRIB FILE : copy a message to FILE

- -ncep_uv FILE : combine U,V into a message like in NCEP operations
- -tosubmsg FILE : create a file with submessages
- -submsg N : process by submessage number

The -tosubmsg N option allows to process by submessage number.
If the N is zero, all the submessages are processed which pretty pointless
as this the default operation. If N is one, then all the messages are processed once.
(Messages with only one field are considered to have one submessage.) The following
will copy from IN.grb to OUT.grb and preserve the submessage structure.

```
$ wgrib2 IN.grb -submsg 1 -GRIB OUT.grb
```

This will copy all the 200 mb fields assuming U/V are in the same message and
keep U/V in the same message.

```
$ wgrib2 IN.grb -submsg 1 -if ":200 mb:" -GRIB 200mb.grb
```

## Usage

```
-submsg N
N is an integer, usually 1
```

### Example

```
$ wgrib2 test.grb2 -submsg 1 -if ":200 mb:" -GRIB 200.grb2 -if ":100 mb:" -GRIB 100.grb2
```

See also: [-ncep_uv](./ncep_uv.md),
[-GRIB](./GRIB.md),
[-tosubmsg](./tosubmsg.md)

---

> Description: misc X process submessage X (0=process all messages)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/submsg.html>_
