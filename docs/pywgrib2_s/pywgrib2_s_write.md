### pywgrib2: write

## Introduction

The routine pywgrib2_s.write(..) writes a grib message to a file or memory file.
To write a grib message, you need a "template", a grib message with
the correct grid and most of the metadata. Often these templates are created interactively
using wgrib2. The procedure is to take an existing grib message, and then modify it to
have the correct grid and metadata.

Once you have a template, pywgrib2_s writes grib files by modifying the template by
adding grid point data, changing the date codes, and other metadata. Pywgrib2_s can
not write a grib file from scratch, you always need a template. Templates are
quite small when you replace the grib point values with zero, with a size often less
than 200 bytes.

### Example

```


```

## Usage

```

     a=pywgrib2_s.write(file,template,msg_no [, optional arguments])

     file     = output file
     template = file containing template
     msg_no   = message number of the template, the template cannot be in a submessage

     Optional Arguments

     new_data = None (default)
                ndarray          use grid point data from numpy's ndarrray
     Append   = False            when opening file, make a new file
                True             when opening file, append to the end of the file
     metadata = ''
              = 'METADATA'       sets metadata to 'METADATA', format is given by
                                 https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/set_metadata.html
     sec0     = None (default)
                byte array       replace grib section 0 with byte array
     sec1     = None (default)
                byte array       replace grib section 1 with byte array
     sec2     = None (default)
                byte array       replace grib section 2 with byte array
     sec3     = None (default)
                byte array       replace grib section 3 with byte array
     sec4     = None (default)
                byte array       replace grib section 4 with byte array
     sec5     = None (default)
                byte array       replace grib section 5 with byte array
     sec6     = None (default)
                byte array       replace grib section 6 with byte array
     sec7     = None (default)
                byte array       replace grib section 7 with byte array
     sec8     = None (default)
                byte array       replace grib section 8 with byte array
                                 useless option, included for completeness
     var      = '' (default)
                'NAME'           set grib variable (ex, 'HGT')
                                   uses wgrib2 naming convention
     lev      = '' (default)
              = 'level'          set level to 'level', ex. '2 m above ground', or '500 mb'
                                   uses wgrib2 naming convention
     time0    = None (default)
              = YYYYMMDDHHmmSS   sets reference time to YYYYMMDDHHmmSS
              = YYYYMMDDHH       sets reference time to YYYYMMDDHH (mmSS=0)
     ftime    = '' (default)
              = 'FTIME'          sets the forecast time, ex '12 hour fcst'
                                   uses wgrib2 naming convention
     packing  = '' (default)     simple
                'same'           use same packing as template
                's'              simple
                'simple'         simple
                'c1'             complex type 1
                'complex1'       complex type 1
                'c2'             complex type 2
                'complex2'       complex type 2
                'c3'             complex type 3
                'complex3'       complex type 3
                'a'              AEC packing
                'aec'            AEC packing
                'j'              jpeg2000
                'jpeg'           jpeg2000
                                 Codes written using the NCEP's g2lib and g2clib do not

     var      = '' (default)
                'NAME'           set grib variable (ex, 'HGT')
                                   uses wgrib2 naming convention
     lev      = '' (default)
              = 'level'          set level to 'level', ex. '2 m above ground', or '500 mb'
                                   uses wgrib2 naming convention
     time0    = None (default)
              = YYYYMMDDHHmmSS   sets reference time to YYYYMMDDHHmmSS
              = YYYYMMDDHH       sets reference time to YYYYMMDDHH (mmSS=0)
     ftime    = '' (default)
              = 'FTIME'          sets the forecast time, ex '12 hour fcst'
                                   uses wgrib2 naming convention
     packing  = '' (default)     simple
                'same'           use same packing as template
                's'              simple
                'simple'         simple
                'c1'             complex type 1
                'complex1'       complex type 1
                'c2'             complex type 2
                'complex2'       complex type 2
                'c3'             complex type 3
                'complex3'       complex type 3
                'a'              AEC packing
                'aec'            AEC packing
                'j'              jpeg2000
                'jpeg'           jpeg2000
                                 Codes written using the NCEP's g2lib and g2clib do not
                                   handle AEC packing.
                                 Codes written using the NCEP's g2lib and g2clib will have
                                   problems with complex packing if they haven't been
                                   programmed to handle undefined grid points without a bitmap.
     pdt     = None (default)
               integer           Product Definition Template (code table 4.0) is set to integer
                                 see https://www.nco.ncep.noaa.gov/pmb/docs/grib2/grib2_doc/grib2_table4-0.shtml
     d_scale = None (default)    decimal scaling for packing
               'same'
               (integer)
     b_scale = None (default)    binary scaling for packing
               'same'
               (integer)
                                 if d_scale and b_scale are not defined, the wgrib2 default value is used
                                      12 bits of binary precision (ECMWF style)
                                 If d_scale or b_scale is set to 'same' then
                                      -set_scaling same same is done.
                                 If d_scale or b_scale are not None
                                      if d_scale is None then d_scale = 0
                                      if b_scale is None then b_scale = 0
                                      -set_scaling d_scale b_scale is done.
                                 If d_scale and b_scale are None, then default wgrib2 scaling is used
                                      This is 12 bits binary in ECMWF style precision.
                                 See https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/
                                          set_scaling.html

```

Order of setting values 2. template

- secN
- pdt
- metadata
- time0, var, lev, ftime, packing

[overview](./pywgrib2_s.html)
[back](./pywgrib2_s_inq.html)
[next](./pywgrib2_s_close.html)

---

> Description: write grib2

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/pywgrib2_s_write.html>_
