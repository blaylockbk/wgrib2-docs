
### wgrib2: -config



### Introduction



The -config option shows the configuration of wgrib2
and then ends the program.

### Usage




```

-config

```

### Example




```

$ wgrib2 -config
wgrib2 v0.1.7.8h 4/2009 Wesley Ebisuzaki, Jaakko Hyvätti, Kristian Nilssen, Karl Pfeiffer, Pablo Romero, Manfred Schwarb, Arlindo da Silva, Niklas Sondell, Sergey Varlamov

Compiled on 10:54:05 Apr 24 2009

Netcdf3 package is installed
Netcdf4 package is not installed
mysql package is not installed
regex package is installed
tigge package is installed
maximum nunber of arguments on command line: 300
stdout buffer length: 30000
g2clib is the default decoder

```


Here is the config four years later.




```

$ wgrib2 -config
bash-4.1$ wgrib2 -config
wgrib2 v0.1.9.9beta3 8/2013 Wesley Ebisuzaki, Reinoud Bokhorst, Jaakko Hyvätti, Dusan Jovic, Kristian Nilssen, Karl Pfeiffer, Pablo Romero, Manfred Schwarb, Arlindo da Silva, Niklas Sondell, Sergey Varlamov

Compiled on 12:09:10 Sep  3 2013

Netcdf package: "3.6.3" of Sep  3 2013 12:01:28 $ is installed
mysql package is not installed
regex package is installed
tigge package is installed
interpolation package is installed
gptpc interface: experimental v0.1
UDF package is not installed
maximum number of arguments on command line: 5000
maximum number of -match,-not,-if, and -not_if arguments: 1000
stdout buffer length: 30000
default decoding: g2clib emulation
g2clib decoders are installed
user gribtable: (none)
C compiler: gcc
Fortran compiler: gfortran
OpenMP: control number of threads with environment variable OMP_NUM_THREADS
INT_MAX:   2147483647
ULONG_MAX: 18446744073709551615

```


See also: 










----

>Description: misc         shows the configuration

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/config.html>_