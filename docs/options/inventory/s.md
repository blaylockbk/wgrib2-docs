# `-s` — Simple Inventory

The `-s` option prints a **simple inventory** of a GRIB2 file, including **byte offsets, date/time, variable name, levels, and forecast time** — with **minute and second precision**.

```console
$ wgrib2 myFile.grib2 -s
1:0:d=2007032600:HGT:1000 mb:anl:
2:125535:d=2007032600:HGT:1000 mb:3 hour fcst:
```

It is functionally equivalent to using the following options together:

```bash
wgrib2 myFile.grib2 -T -var -lev -ftime -misc yyppp
```

## Save the Inventory to a File

It is common practice to save the inventory output to an index file using the `.idx` suffix:

```console
$ wgrib2 new.grib2 -s > new.grib2.idx
1:0:d=2007032600:HGT:1000 mb:anl:
2:125535:d=2007032600:HGT:1000 mb:3 hour fcst:
```

---

## Usage

```text
-s
```

---

## Related Options

See also:

- [Macros](./macros.md)
- [`-match_inv`](./match_inv.md)

---

!!! note "**Description:** Simple inventory with minute and second precision _Subject to change_"

_Documentation derived from: [https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/S.html](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/S.html)_
