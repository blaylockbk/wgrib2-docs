# wgrib2: -for_n

A grib file has set of grids and you can reference the field by its grib message number and submessage number.
You can also reference the field by its inventory number which starts at 1.
The -for_n option allows you to process a subset of the
grib file using a for-loop syntax on the inventory number. Suppose you want to process
grids 10 through 20 by 2, you can add the option -for_n 10:20:2.

## Usage

```

-for_n I:J:K        same as for n = I to J by K
-for_n I:J          same as for n = I to J by 1
-for_n I::K         same as for n = I to MAX_INTEGER by K
-for_n I            same as for n = I to MAX_INTEGER by 1

```

# wgrib2: multi-processing with -for_n and -n

## Introduction

There are two ways to parallize wgrib2, one way is
the thread the loops and another is to run multiple copies
of wgrib2. Threading the loops is great but there is
so much serial code that the speed up is limited. The
biggest speed up can come from running multiple copies
of wgrib2. a Rather than pull out the MPI textbook,
we'll show some script-level multiprocessing.

### Assumptions

- CPU time is longer than the I/O time
- each record can be handled independantly
- multiple cpus are available on the same machine/node
- a two cpu version is sufficient documentation

### The inventory number, -n

Our first step is to understand the inventory number.
You can see the inventory number by the -n option.
Once we have add the inventory number, we can have one copy of wgrib2
process the even numbered and another process the odd numbered records.

Note that the inventory number is not the same as the
record number for many reason such as the order of processing
may be read from standard input by -i,
some messages may have submessages and some records could
be skipped by the -match and other options.

### Even and Odd, -for_n

The -for_n option is like the
-for option except that it uses the inventory
number rather than the record number.

To select the odd records to process, you use the
option -for_n 1:99999:2. Here, 99999 is just
a large number greater than the number of records.
You could also use -for_n 1::2.
To process the even fields, use -for_n 2::2.

### Pipes, fifo (Unix/Linux)

Now that we can run wgrib2 on the even and odd records, how
do we make the output. Here is a simple way in Unix/Linux.

```

# bad example: slow, output in different order
# input=grib file, output=grib file, makes regional subset using two copies of wgrib2
f=input
wgrib2 $f -ijsmall_grib 1:10 1:10 /tmp/p1 -for_n 1:99999:2 >/dev/null
wgrib2 $f -ijsmall_grib 1:10 1:10 /tmp/p2 -for_n 2:99999:2 >/dev/null
cat /tmp/p1 /tmp/p2 >output
rm /tmp/p1 /tmp/p2

```

The above method is not optimal as it uses temporary files,
rearranges the order of the records and is limited to 99999 records. A better method is to use
pipes and a simple program that reads the pipes and writes out
a merged output file.

```

# input=grib file, output=grib file, makes regional subset using two copies of wgrib2
# uses named pipes for speedup, keeps order of records
f=input
mkfifo /tmp/p1.$$
mkfifo /tmp/p2.$$
wgrib2 $f -ijsmall_grib 1:10 1:10 /tmp/p1.$$ -for_n 1::2 >/dev/null &
wgrib2 $f -ijsmall_grib 1:10 1:10 /tmp/p2.$$ -for_n 2::2 >/dev/null &
gmerge output /tmp/p1.$$ /tmp/p2.$$
rm /tmp/p1.$$ /tmp/p2.$$

```

The program, gmerge, simply reads a grib message from p1.$$ and
writes the output to "output".
Then it reads a grib message from p2.$$ and writes
it to "output". This is repeated until there is no data left (pipes are closed).
gmerge is part of the wgrib2 distribtution.

The program amerge is like gmerge except it reads one line from p1
and writes it to the output. Then it reads one line from p2 and
writes it to output. This is repeated until there is no data left
(pipes are closed). The amerge program can be used to run
inventories on mutiple cpus.

```

# input=grib file, run 2 copies of wgrib2 and writes inventory on stdout
f=input
mkfifo /tmp/p1.$$
mkfifo /tmp/p2.$$
wgrib2 -for_n 1::2 $file -s -lon 0 0 >/tmp/p1.$$ &
wgrib2 -for_n 2::2 $file -s -lon 0 0 >/tmp/p2.$$ &
amerge /tmp/p1.$$ /tmp/p2.$$
rm /tmp/p1.$$ /tmp/p2.$$

```

### More than 2 CPUs

The previous examples split the work into two processes. This
works well for a dual core system. On systems with 4 cores,
you may want to split the work into 4 jobs. The current versions
of gmerge and amerge allow input for upto 32 different inputs.

```

# same as 2nd example but with 3 cpus
f=input
mkfifo /tmp/p1.$$ /tmp/p2.$$ /tmp/p3.$$
wgrib2 $f -ijsmall_grib 1:10 1:10 /tmp/p1.$$ -for_n 1::3 >/dev/null &
wgrib2 $f -ijsmall_grib 1:10 1:10 /tmp/p2.$$ -for_n 2::3 >/dev/null &
wgrib2 $f -ijsmall_grib 1:10 1:10 /tmp/p3.$$ -for_n 3::3 >/dev/null &
gmerge output /tmp/p1.$$ /tmp/p2.$$ /tmp/p3.$$
rm /tmp/p1.$$ /tmp/p2.$$ /tmp/p3.$$

```

### Source code for gmerge and amerge

[gmerge](https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2_aux_progs/gmerge/): merge grib files (1 grib message at a time)

[amerge](https://ftp.cpc.ncep.noaa.gov/wd51we/wgrib2_aux_progs/amerge/): merge ascii files (1 line at a time)

## Usage

```

-n                  prints the inventory number
-for_n I:J:K        same as for n = I to J by K
-for_n I:J          same as for n = I to J by 1
-for_n I::K         same as for n = I to MAX_INTEGER by K
-for_n I            same as for n = I to MAX_INTEGER by 1

```

See also:
[-if_n](./if_n.html),
[-for](./for.html),
[-flush](./flush.html),
[-n](./n.html),
[using pipes](./pipes.html),

---

> Description: init X process inv numbers in range, X=(start:end:step), only one -for allowed

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/for_n.html>_
