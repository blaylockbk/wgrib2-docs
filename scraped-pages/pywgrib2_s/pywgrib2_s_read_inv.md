### pywgrib2: read_inv

## Introduction

The routine pywgrib2_s.read_inv(inv) reads an inventory file, inv. It
returns a list of the entries of the inventory file. The
inventory file can be a memory file or regular file.

### Example

```

>>> pywgrib2_s.mk_inv('a.grb','a.inv')
0
>>> a=pywgrib2_s.read_inv('a.inv')
>>> print(a[0])
1:0:D=20200101000000:CDCON:high cloud layer:0-6 hour ave fcst::CDCON:n=1:npts=18048:var0_1_0_7_6_2:pdt=8:d=2020010100:
start_FT=20200101000000:end_FT=20200101060000:scaling ref=0 dec_scale=0 bin_scale=0 nbits=6:vt=2020010106:
>>> print(a[1])
2:2808:D=20200101000000:CDCON:middle cloud layer:0-6 hour ave fcst::CDCON:n=2:npts=18048:var0_1_0_7_6_2:pdt=8:d=202001
0100:start_FT=20200101000000:end_FT=20200101060000:scaling ref=0 dec_scale=0 bin_scale=0 nbits=6:vt=2020010106:

```

## Usage

```

     a=pywgrib2_s.read_inv(inv_file)
         if inv_file cannot be read, returns an empty list.  Otherwise
         returns the inventory file as a list.  The length the list
         (len(a)) is the number of lines in inv_file.

```

[overview](./pywgrib2_s.html)
[back](./pywgrib2_s_mk_inv.html)
[next](./pywgrib2_s_inq.html)

---

> Description: reads inventory file, returns list

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pywgrib2_s_read_inv.html>_
