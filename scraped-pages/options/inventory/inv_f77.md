# wgrib2: -inv_f77

## Introduction

The -inv_f77 option writes the match inventory
(see the -match_inv option) to a file
that is readable by a fortran unformatted I/O statement. The
format is more or less standand and consistes of a 4-byte integer
of number of bytes in the character string,
the character string and 4-byte integer with the length of the
character string as a trailer. If the match inventory is shorter
than the character string, it is padded with blanks. If the
match inventory is longer, it simply truncated. Not all fortran
compilers support this format.

## Usage

```
 -inv_f77 OPTION CLEN FILE

   OPTION = "ieee" or "bin"
      bin = native format
      ieee = 4-byte big_endian header/trailer (default)
             4-byte little_endian header/trailer (by option)
   CLEN = integer, the length of the character string
   FILE = output file

```

### Example

```

  wgrib2 IN.grb -inv_f77 bin 100  inv.dat

  character*100  m_inv
  open(unit=11,file='inv.dat',form='unformatted')
  read(11) m_inv
  write(*,*) 'first record:',trim(m_inv)
  read(11) m_inv
  write(*,*) 'second record:',trim(m_inv)

```

The -inv_f77 option was added to facilitate the
writing of fortran codes that can read wgrib2 output.

See also:
-match_inv

---

> Description: inv> X Y Z match inventory written to Z with character\*(Y) and X=(bin,ieee)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/inv_f77.html>_
