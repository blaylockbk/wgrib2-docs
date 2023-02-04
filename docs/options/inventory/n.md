# wgrib2: -n

## Introduction: message number vs inventory number

A grib file is made of messages and each message can contain one or more gridded fields.
If the message has two or more gridded fields, we say the message has submessages.
The wgrib2 convention for number the messages/submessages is

```

Message/submessage numbering convention:

   No submessage, one field

     I     I is integer starting from 1 and is the message number

   Submessage, multiple fields

     I.J   I is integer starting from 1 and is the message number
           J is integer and is the sub-message number
           J = 1 for the first submessage of the Ith message


Inventory numbering convention:

     I     I is integer starting from 1 and is the number of the field

```

An alternative method for numbering the fields is the inventory number. The inventory number
is simply the grids starting from one. The inventory number is simply the line number of
the default wgrib2 inventory. The message number is the first column of the the default
wgrib2 inventory.

Both the message number and the inventory number are valid ways of numbering the fields in
a grib file. The message number is the original method and reflects the structure of
the grib message/submessage structure. The inventory number is a logical numbering scheme
when trying trying to multitask wgrib2. (For example, send even fields to CPU1 and odd fields
to CPU2.)

```

-sh-2.05b$ ./wgrib2 test.grb2 -s -npts -d 1
1:0:d=2005090200:HGT:1000 mb:60 hour fcst:npts=259920

```

## Usage

```

-n

```

See also:
[-if_n](./if_n.html),
[-for_n](./for_n.html)

---

> Description: inv prints out inventory number

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/n.html>_
