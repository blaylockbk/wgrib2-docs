# GRIB2 Inventory

The `wgrib2` utility provides capabilities for inspecting the contents of GRIB2 files without extracting the actual data. This is essential for understanding what variables, levels, and forecast times are contained in a GRIB2 file before processing. These are commonly called an "index" or "inventory" of the GRIB2 file.

## Default Inventory

The simplest way to list a GRIB2 contents is to run `wgrib2` with just the filename:

```bash
wgrib2 filename.grb2
```

which would output something like the following:

```
1:0:d=2024120800:TMP:2 m above ground:anl:
2:12345:d=2024120800:RH:2 m above ground:anl:
3:45678:d=2024120800:UGRD:10 m above ground:anl:
```

These `:`-separated values the following for each record in the GRIB2 file:

- **Record number**: Sequential message number in the file
- **Byte offset**: Starting position of each message
- **Date**: Reference time of the data in YYYYMMDDHH format
- **Variable**: Parameter name
- **Level**: Vertical level information
- **Forecast time**: Valid time or forecast hour

You will find that the following are equivalent:

```bash
wgrib2 filename.grib2
wgrib2 -s filename.grib2
wgrib2 -t -var -lev -ftime -misc filename.grib2
```

### Inventory of a real HRRR file

Let's get and inspect an index file from the High-Resolution Rapid Refresh (HRRR) model:

```bash title="download a HRRR file from Amazon Web Services"
wget https://noaa-hrrr-bdp-pds.s3.amazonaws.com/hrrr.20250102/conus/hrrr.t00z.wrfsfcf00.grib2
```

Since there are many records, lets just list the first five:

```console
$ wgrib2 ./hrrr.t00z.wrfsfcf00.grib2 | head -n 5
1:0:d=2025010200:REFC:entire atmosphere:anl:
2:381614:d=2025010200:RETOP:cloud top:anl:
3:564153:d=2025010200:var discipline=0 center=7 local_table=1 parmcat=16 parm=201:entire atmosphere:anl:
4:1089819:d=2025010200:VIL:entire atmosphere:anl:
5:1476545:d=2025010200:VIS:surface:anl:
```

## Create an index file

If you are producing or distributing GRIB2 files, it is best practice to also provide a simple inventory using the same filename with the `.idx` suffix.

```bash
wgrib2 -s filename.grib2 > filename.grib2.idx
```

## Verbose Inventory (-V)

For detailed metadata about each record, use `-V`:

```bash
wgrib2 -V filename.grb2
```

Output includes:

- Grid definition
- Statistical processing information
- Ensemble member details
- Additional metadata fields

For our HRRR file:

```console
$ wgrib2 -V ./hrrr.t00z.wrfsfcf00.grib2 | head -n 9
1:0:vt=2025010200:entire atmosphere:anl:REFC Composite reflectivity [dB]:
    ndata=1905141:undef=0:mean=-7.47602:min=-10:max=53.5
    grid_template=30:winds(grid):
        Lambert Conformal: (1799 x 1059) input WE:SN output WE:SN res 8
        Lat1 21.138123 Lon1 237.280472 LoV 262.500000
        LatD 38.500000 Latin1 38.500000 Latin2 38.500000
        LatSP 0.000000 LonSP 0.000000
        North Pole (1799 x 1059) Dx 3000.000000 m Dy 3000.000000 m

```

### Match Specific Variables (-match)

Use the `-match` option to filter the inventory for only specific variables.

```console title="Show all temperature records"
$ wgrib2 hrrr.t00z.wrfsfcf00.grib2 -match "TMP"
15:8303112:d=2025010200:TMP:500 mb:anl:
20:11626595:d=2025010200:TMP:700 mb:anl:
26:15543517:d=2025010200:TMP:850 mb:anl:
30:18468349:d=2025010200:TMP:925 mb:anl:
34:21454096:d=2025010200:TMP:1000 mb:anl:
64:32817321:d=2025010200:TMP:surface:anl:
71:36954241:d=2025010200:TMP:2 m above ground:anl:
```

```console title="Wind and temperature records at 200 and 500 mb"
$ wgrib2 hrrr.t00z.wrfsfcf00.grib2 -match ":(UGRD|VGRD|TMP):(700|500) mb:"
15:8303112:d=2025010200:TMP:500 mb:anl:
17:9723023:d=2025010200:UGRD:500 mb:anl:
18:10316390:d=2025010200:VGRD:500 mb:anl:
20:11626595:d=2025010200:TMP:700 mb:anl:
23:13614254:d=2025010200:UGRD:700 mb:anl:
24:14213777:d=2025010200:VGRD:700 mb:anl:
```

### Filter by Level (-lev)

Show only records at specific levels:

```bash
wgrib2 -lev "2 m" filename.grb2
wgrib2 -lev "850 mb" filename.grb2
```

### Combination Filtering

Combine options for precise searching:

```bash
wgrib2 -s -match "TMP" -lev "2 m" filename.grb2
```

## Advanced Inventory Techniques

### Count Records

Count the total number of records in a file:

```bash
wgrib2 filename.grb2 | wc -l
```

### Search and Extract Information

Use grep to find specific information:

```bash
# Find all 500mb records
wgrib2 -s filename.grb2 | grep "500 mb"

# Find all temperature variables
wgrib2 -s filename.grb2 | grep ":TMP:"

# Find specific forecast hours
wgrib2 -s filename.grb2 | grep "3 hour fcst"
```

### Custom Output Format (-v)

Create custom inventory formats:

```bash
wgrib2 -s -var -lev -ftime filename.grb2
```

This shows only variable name, level, and forecast time for each record.

## Practical Use Cases

### Case 1: Quick File Overview

Before processing a new GRIB2 file, get a quick overview:

```bash
wgrib2 -s filename.grb2 | head -20
```

Shows the first 20 records to understand file structure.

### Case 2: Find Specific Data

Locate a specific variable at a specific level:

```bash
wgrib2 -s filename.grb2 | grep "TMP" | grep "850 mb"
```

### Case 3: List All Variables

Get a unique list of all variables in the file:

```bash
wgrib2 -s filename.grb2 | cut -d: -f4 | sort -u
```

### Case 4: List All Levels

Identify all vertical levels present:

```bash
wgrib2 -s filename.grb2 | cut -d: -f5 | sort -u
```

### Case 5: Check Forecast Times

See all forecast times available:

```bash
wgrib2 -s filename.grb2 | cut -d: -f6 | sort -u
```

## Output Format Reference

### Standard Format Fields

The `-s` option produces colon-separated fields:

```
record:byte_offset:reference_date:variable:level:forecast_info:
```

| Field | Description    | Example                      |
| ----- | -------------- | ---------------------------- |
| 1     | Record number  | `1`                          |
| 2     | Byte offset    | `0`                          |
| 3     | Reference date | `d=2024120800`               |
| 4     | Variable name  | `TMP`, `RH`, `UGRD`          |
| 5     | Level          | `2 m above ground`, `500 mb` |
| 6     | Forecast time  | `anl`, `3 hour fcst`         |

## Performance Tips

!!! note "Performance Considerations" - Inventory operations are fast since they only read GRIB2 headers, not data - Use `-match` and `-lev` to reduce output for large files - Pipe output to `less` for easier navigation: `wgrib2 -s file.grb2 | less`

## Common Pitfalls

!!! warning "Common Issues" - **Case sensitivity**: Variable names and match patterns are case-sensitive - **Level formats**: Level descriptions vary (e.g., "850 mb" vs "850 MB") - **Regex patterns**: Use proper regex syntax with `-match` option

## Additional Resources

For more advanced wgrib2 usage:

```bash
wgrib2 -help        # Display all available options
wgrib2 -help long   # Detailed help for all options
```

## Summary

Quick reference for the most useful inventory commands:

| Command                                 | Purpose                  |
| --------------------------------------- | ------------------------ |
| `wgrib2 file.grb2`                      | Basic inventory          |
| `wgrib2 -s file.grb2`                   | Short detailed inventory |
| `wgrib2 -V file.grb2`                   | Verbose metadata         |
| `wgrib2 -match "VAR" file.grb2`         | Filter by variable       |
| `wgrib2 -lev "LEVEL" file.grb2`         | Filter by level          |
| `wgrib2 -s file.grb2 \| grep "PATTERN"` | Search inventory         |
