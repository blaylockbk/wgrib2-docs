---
title: Home
---

![](_static/wgrib2-logo.png)

**wgrib2** is a utility for processing and manipulating GRIB files. The utility was designed to be used to reduce the need for custom Fortran programs to read, write and manipulate GRIB files.

<div class="grid cards" markdown>

- :material-github: **wgrib2 on GitHub**  
  Source code, issues, and releases.  
  [View repository →](https://github.com/NOAA-EMC/wgrib2){target=\_blank}

- :material-file-document-outline: **Doxygen Docs**  
  Auto-generated API documentation.  
  [View docs →](https://noaa-emc.github.io/wgrib2/){target=\_blank}

- :material-archive: **Legacy Docs**  
  Original CPC documentation.  
  [View legacy site →](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/){target=\_blank}

- :material-youtube: **EMC wgrib2 Seminar**  
  Video presentation on wgrib2.  
  [Watch on YouTube →](https://www.youtube.com/watch?v=udbxfC1V2DI)

</div>

**wgrib2** has the following abilities:

- inventory and read grib2 files
- create subsets
- create regional subsets by cookie cutter or projections
- export to ieee, text, binary, CSV, netcdf and mysql
- import to ieee, text, binary, and netcdf
- write of new grib2 fields
- parallel processing by using threads (OpenMP)
- parallel processing by flow-based programming

!!! note

    This is a remake of the original [wgrib2 documentation](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/). I scraped the documentation HTML pages, converted them to Markdown, rebuilt the pages with Sphinx, and hosted them on Read the Docs. These pages are **unofficial** documentation for wgrib2.

    I primarily did this because it's sometimes hard to find the right page in the official docs and I remade the docs for my own benefit.

!!! tip "Get Involved"

    I would love your help to make *the best wgrib2 documentation ever*. Start by submitting an Issue or Pull Request to [wgrib2-docs](https://github.com/blaylockbk/wgrib2-docs).
