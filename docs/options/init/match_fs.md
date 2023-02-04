
### wgrib2: -match\_fs, -not\_fs, -if\_fs, -not\_if\_fs



### Introduction



The (-match\_fs, -not\_fs, -if\_fs, -not\_if\_fs) options are 
similar to the 
(-match, -not, -if, -not\_if) options except the former
group uses fixed strings rather than regular expressions. 


Now why would you want to use the "fs" versions? Suppose you want to to
search for the "2.5 mb" level.


```

bash-4.1$ wgrib2 -match ":2.5 mb:" junk
1:0:d=2009060500:HGT:225 mb:180 hour fcst:ENS=+19
2:46042:d=2009060500:HGT:2.5 mb:180 hour fcst:ENS=+19

```


What happened? The command matched the 2.5 and 225 mb level! Well the
period is regular expression metacharacter and matchs any character. To
only get "2.5 mb" either have to quote the period or change the regex mode.


```

bash-4.1$ wgrib2 -match ":2\.5 mb:" junk
2:46042:d=2009060500:HGT:2.5 mb:180 hour fcst:ENS=+19

bash-4.1$ wgrib2 -set_regex 0 -match ":2.5 mb:" junk
1:0:d=2009060500:HGT:225 mb:180 hour fcst:ENS=+19
2:46042:d=2009060500:HGT:2.5 mb:180 hour fcst:ENS=+19

bash-4.1$ wgrib2 -set_regex 1 -match ":2.5 mb:" junk
2:46042:d=2009060500:HGT:2.5 mb:180 hour fcst:ENS=+19

bash-4.1$ wgrib2 -set_regex 2 -match ":2.5 mb:" junk
2:46042:d=2009060500:HGT:2.5 mb:180 hour fcst:ENS=+19

```


So setting the regex mode to 1 or 2 will work. The "\_fs" options 
uses fixed strings with no metacharacters which is the same
as the regex mode set to 1.

The "\_fs" options only requires standard C support rather than POSIX-2
so they are available on all wgrib2. Options using
regular expressions are optional and may not be available on platforms.


See also: 
[-if](./if.html),
[-if\_fs](./if_fs.html),
[-match](./match.html), 
[-match\_inv](./match_inv.html), 
[-not](./not.html), 
[-not\_fs](./not_fs.html), 
[-not\_if](./not_if.html),
[-not\_if\_fs](./not_if_fs.html),
[-set\_regex](./set_regex.html).










----

>Description: init  X      process data that matches X (fixed string)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/match_fs.html>_