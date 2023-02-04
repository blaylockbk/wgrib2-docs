# wgrib2: -if_rec

## Introduction

The -if_rec option is an -if for a range of record numbers.
The -if_rec option uses the same syntax as the "for" option.
If you want to operate on
records 10 to 20, you would use the parameter 10:20.
If you want to operate on all the even records from 10 to 20, you would
use 10:20:2. The restrictions are the start value must be less than
the ending value and the step has to be greater than zero.

The -if_rec option is similar to the
-for option in that they both select a range of records.
The difference is that the -for option selects the range
of records that wgrib2 will process. With the
the -if_rec option, all the records will be processed
but only the additional processing within the -if block will be only
done for the selected records. The -for option can be much
faster if the field has to be decoded. With the -for option,
only the selected records need to be docoded whereas all the records would be decoded
when using the -if_rec option. The command line, however,
can have multiple -if_rec options.

## Usage

```

-if_rec I:J:K        same as for n = I to J by K
-if_rec I:J          same as for n = I to J by 1
-if_rec I::K         same as for n = I to MAX_INTEGER by K
-if_rec I            same as for n = I to MAX_INTEGER by 1

```

### Example

```

 $ wgrib2 file.grb2 -if\_rec 4:5 -s -fi
4:13335:d=2008120200:RH:750 mb:anl:
5:17098:d=2008120200:TMP:2743 m above mean sea level:anl:

```

See also: [-match](./match.html)
[-if_n](./if_n.html)
[-for](./for.html)
[-for_n](./for_n.html)

---

> Description: if X if (record numbers in range), X=(start:end:step)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/if_rec.html>_
