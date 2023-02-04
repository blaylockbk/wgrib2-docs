### wgrib2api: grb2_wrt(..)

## Introduction

Writing grib using wgrib2api is simple. You need

1. gridded data you want to write

- sample grib2 message with the same grid as (1) and unchanging
  metadata such as center/subcenter/process-id
- the changing metadata in a wgrib2-style string such as

'd=1999123100:HGT:500 mb:anl:', 'D=20170102123000:UGRD:2 m above ground:15 minute fcst:'

The grb2_wrt(..) function will
take the template, change the grid values, change the grib headers based
on the metadata string and write out the grib message.

Optional parameters that are in the grb2_wrt source code that
are not documented here are to be considered to be alpha
code.

### A simple fortran program to write grib2

```

use wgrib2api
real, allocatable :: grid(:,:)

allocate (grid(360:181))
read(11) grid
i = grb2_wrt('out.grb2','tempplate.grb2',1,data2=grid,meta='D=20170102030000:HGT:500 mb:anl:')
write(*,*) 'error=',i
stop
end

```

### A simple program to write out the 10 m wind speed

```

use wgrib2api
real, allocatable :: u(:,:), v(:,:)
character (len=200) meta

i=grb2_mk_inv('gep19.t00z.pgrb2af180','in.inv')                    ! make index file
if (i.ne.0) stop 1
i=grb2_inq('gep19.t00z.pgrb2af180','in.inv', ':UGRD:10 m above ground:', data2=u) ! read U
if (i.ne.1) stop 2                                                 ! only want one match
i=grb2_inq('gep19.t00z.pgrb2af180','in.inv', ':VGRD:10 m above ground:', data2=v,& ! read V
     desc=meta)
if (i.ne.1) stop 3                                                 ! only want one match

u=sqrt(u*u+v*v)                                                    ! calculate wind speed
i=grb2_wrt('out.grb','gep19.t00z.pgrb2af180',1,data2=u,meta=meta,var='WIND')  ! write wind speed
! using grib message 1 of original file as a template
! modifying the variable to WIND (wind speed
if (i.ne.0) stop 4
write(*,*) ' file : out.grb'
stop
end

```

### Code fragment to write surface hgt from ss2grb2.f90

```

  metadata='d=' // datecode // ':HGT:surface:' // trim(ftime) // ':'
  iret = grb2_wrt(grib_output,grib_template,1,hgt_sfc,meta=metadata,order='raw')
  if (iret.ne.0) stop 1
  write(*,*) 'grib_write HGTsfc'

```

The grib template, grib_template, is created on the fly by using wgrib2
to alter the grid.

In ss2grb2, the grids are stored in we:ns order and we want the output grids
to be in we:ns order. So the order parameter is set to raw. The order could
have been set to we:ns but raw is slightly faster.

### Code fragment to write hgt(pres) from ss2grb2.f90

```

  do i = 1, n_plevs
     metadata='d=' // datecode // ':HGT:' // trim(plevs_txt(i)) &
       // ':' // trim(ftime) // ':'
     iret = grb2_wrt(grib_output,grib_template,1,xz(:,i),meta=metadata,order='raw')
     if (iret.ne.0) stop 4
  enddo
  write(*,*) 'grib_write HGT mb'

```

The pressure levels are defined by both a numeric value (plevs(n_plevs)
and a text string (plevs_txt(n_plevs)). Note that the loop that writes
calls grb2_wrt(..) is not multithreaded (no !$OMP PARALLEL DO) because
the wgrib2api is not thread safe.

You may notice that both code fragments use the old style where you modify
the metadata. The new style wasn't available for ss2grb2.f90.

### Usage

```

    iret =     grb2_wrt(GRB2, TEMPLATE, IMSG, data2=GRID, meta=META, (list of optional arguments))
               or
    iret =     grb2_wrt(GRB2, TEMPLATE, IMSG, data1=GRID1, meta=META, (list of optional arguments))

    iret:      integer
               0 grib message written
               1 grib message not written

               Necessary Parameters

    GRB2:      character (len=*) output grib file
    TEMPLATE:  character (len=*) template file
    IMSG:      integer, grib message number to be used as template
    data2:     real allocatable :: grid(nx:ny)          : modern programs
               values of grid
    data1:     real grid1(nx*ny)                        : legacy programs
               values of grid
    meta:      character (len=*) metadata string, modeled on wgrib2 -S

               Optional Parameters

    meta:      is optional, concise form of the metadata
    append:    integer
               0     create GRB2
               /= 0  append to GRB2
    encode_bits: N, integer
               use ECMWF-style scaling (wgrib2 default)
               grid values = I * 2**M + offset, where I = 0..2^N - 1
               I has at most N bits
               wgrib2/wgrib2api supports N as large as 25.
    debug:     integer
               0     no debug information
               /= 0  debug information
    order:     character (len=*)
               'we:sn'             grid data is we:sn order  (DEFAULT)
               'we:ns'             grid data is we:ns order
               'raw'               grid data is raw order

               Parameters that Override Values in Metadata String (v2.0.7)

    var:       character (len=*)
               ex. 'HGT', 'TMP'
    date:      integer (kind=8)
               sets the reference date, format is YYYYMMDDHHmmss
    level:     character (len=*)
               wgrib2 style level definition, ex. '10 m above ground', '50 mb'
    timing:    character (len=*)
               wgrib2 style forecast type, ex. 12 hour fcst, 0-6 hour ave fcst

    center:    integer, 0..255
               WMO defined center id, 255=undefined
    subcenter: integer, 0..255
               subcenter id, 255=undefined
    packing:   character (len=*)
               wgrib2 style packing name, ex c1, aec, simple

```

---

> Description: grib2_wrt(..)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/grb2_wrt.html>_
