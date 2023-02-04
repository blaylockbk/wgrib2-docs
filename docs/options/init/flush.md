# wgrib2: -flush

## Introduction

When the flush mode is off, output is buffered. Than means the output
is saved to a memory buffer and is only flushed (written out) when
the buffer is full or the program ends. This mode speeds up the output.
However, this mode fails when writting to a pipe or file and another program
is reading from that pipe or file while wgrib2 is executing.

The -flush option causes wgrib2 to flush the
output buffers after every write. This option is now rarely used
because wgrib2 internally sets the flush option on when detects a
write to a pipe. The only current need for the
-flush option is when another program
is reading the disk file while wgrib2 is writting that file.
In this case, you would use this option to ensure that the
disk file is written as soon as possible.

In systems that do not have a POSIX-compatible stat() function, the flush
mode is turned on.

## Usage

```
-flush
```

---

> Description: init flush output buffers after every write (interactive)

_Docs derived from <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/flush.html>_
