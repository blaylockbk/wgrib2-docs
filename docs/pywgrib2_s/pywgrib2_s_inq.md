### pywgrib2: inq(..)

## Introduction

Pywgrib_s.inq(..) does an inquiry about a grib file. At its most basic level,
you give zero or more search terms, and the inquiry says how many fields matched
your search terms. You can also get the metadata and grid information for all the matched
fields. For the last match, you can get additonal information like,

1. nx number of grid points in longitudinal direction

- ny number of grid points in latitudinal irection
- ndata number of grid points in grid
- msgno grib messsage number
- submsgno grib submessage number
- data (optional) grid point values in WE:SN order
- lon (optional) longitude values of grid in WE:SN order
- lat (optional) latitude values of grid in WE:SN order

### Arguments to inq()

### File to Read: Grib file

The function, inq, has only one positional parameter, a name of a grib file.
All other parameters are optional. If you call inq with no options, then
there are no constraints of the fields that match. The call will return the
number of fields in the file.

```

>>> import pywgrib2_s
finished loading libraries
pywgrib2_s v0.0.10 11-9-2020 w. ebisuzaki
>>> pywgrib2_s.inq('a.grb')
9796
>>>

```

### Specifying Fields to Examine

### By Search Terms: Text Searchs

After the grib file, you can enter any number of text search terms.
The search terms will be compared with the inventory file or automatically
generated inventory file if an inventory is not provided.

Each line of the inventory file corresponds to a grib message or submessage.
For a search to be sucessful, each search term must be found in the same line.
The comparison is done as a text string unless the option Regex is set to True.
In that case, a regular expression comparison is done.

### By Record number, byte location: select='select string'

The optional select='select string', allows one to select a specific field by grib message
number or byte location. Since a grib message may have submessages, you may have
to indicate which submessage is being selected. The byte location option is faster
because the routine doesn't have to read from the start of the file to find the selected
grib message.

You can select the grib message to process by the select string option.
The format of the string is

- 'N.M' where N is the grib message number and M is the submessage number
- 'N' where N is the grib message number (M is assumed to be one)
- 'N.M:byte_location where the byte location of the message starting from zero, N is unused
- 'N:byte_location where the byte location of the message starting from zero and M is one

### Faster with inv file

### Optional: inv='inv file name'

The inv file is a human readable index file. With experience,
the inventory becomes [comprehensible](./default_inv.md).
If a inv file is specified, the inventory file will be used to find
the matching messages. This is much faster than reading the grib
file to find the matching grib messages.
You can use any 1-line/field wgrib2 inventory for your "inv file".
However, it is recommended that you use the inventory from the
-Match_inv option from wgrib2 because then the match results with/without
an inventory file will be the same.

### Inquires: What's in selected fields

### Grid point values: Data=Logical

If Data is True, the grid point values of the last match are saved
in pywgrib2_s.data as a numpy ndarray in WE:SN order.
The default value is Data=False. If no grid point values are saved,
the value of pywgrib2_s.data is None. Currently the ndarray uses
float32 but that will change to float64 if/when wgrib2 moves to float64
data.

### Latitude, Longitude for grid points: Latlon=Logical

If Latlon is True, the latitude and longitude values of the last match are saved
in pywgrib2_s.lat and pywgrib2_s.lon as a numpy ndarray in WE:SN order.
The default value is Latlon=False. If the locations are not requested
or cannot be calculated, the value of pywgrib2_s.lat and pywgrib2_s.lon are None.
Currently the ndarray uses float32 but that will change to float64 in the future.

### Contents of Grib Sections: SecN=Logical, N=0..8

A grib message (record) consists of sections 0..8. If SecN is True, the
byte values of SecN are stored in pywgrib2_s.secN. The default value
of SecN is False. Using the data in pywgrib2_s.secN requires a knowledge
about grib.

### List of the matched fields: Matched=Logical

If Matched is True, a list of the fields matched are saved in
pywgrib2_s.matched. The number of matches is given by the
return value if pywgrib2_s.inq(..) and by len(match) when Matched is True.
The default value of Matched is False.

### List of the grid definitions of the matched fields: Grid_defn=Logical

If Grid_defn is True, a list of the grid definitinos of the matched fields is
saved as a list in pywgrib2_s.grid_defn. The number of matches is given by the
return value if pywgrib2_s.inq(..) and by len(grid_defn) when Grid_defn is True.
The default value of Matched is False.

### Saving the matched fields

### Save matched fields in grib: grib='output grib file'

If grib is not '', the grib messages that are matched are copied to the 'output grib file'.
If the 'output grib file' has been not opened, the default is to create a new file.
If the input grib message has submessages, only the submessage that is matched is written
to the file grib.

### Open 'grib file' as append: Append_grib=Logical

If you wish to append grib message to an existing 'output grib file',
you need to add the parameter Append_grib=True. The default value is False.
Otherwise the initial open of 'output grib file' will create a new file.

### Saving matched fields in binary: bin='binary file'

If bin is not '', the grid point data will be written to 'binary file'. The format is the native
single precision with no headers or trailers. The data will be written in WE:SN order.
If the 'binary file' has not been opened, the default is to create a new file.

### Open 'binary file' as append: Append_bin=Logical

If you wish to append binary data to an existing 'binary file',
you need to add the parameter Append_bin=True. The default value is False, and
the initial open of 'binary file' will create a new file.

### Misc

### Text search terms are regular expressions: Regex=Logical

By default, the text search is done using "fixed strings". The search is
done using no wildcards or metacharacters like the fgrep utility. If Regex is set to True,
the search term is a "regular" expression like the egrep utility.

### Read/Search sequentially: Sequential=integer

Often you need to process a file sequentially; for example, copy all the
temperatures fields to another file. Sometimes, you want to process a file
sort of sequentially; for example, find the next zonal wind, find the
corresponding meridional wind and write both files to a file, and repeat
until there are now more zonal winds. Pywgrib2_s handles the second
set which is a superset of the first.

Pywgrib2_s.write(..) can read a inventory file sequentially. To handle
the above second case, you create two inventory files. The first inventory
file is read sequentially. The second inventory file is to find the associated
fields.

To start a sequential read, you add "sequential=0" in the call to pywgrib2_s.write(..).
For the remainder of the sequential reads, "sequential=N" where N is a integer greater than zero.

Add more text here.

[overview](./pywgrib2_s.md)
[back](./pywgrib2_s_read_inv.md)
[next](./pywgrib2_s_write.md)

---

> Description: inquire about grib2 file, read metadata, grid point data, calculate lat-lon of grid points for common grids

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pywgrib2_s_inq.html>_
