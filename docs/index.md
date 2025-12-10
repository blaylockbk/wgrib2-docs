---
title: Home
---

<div style="text-align: center; margin: 2em 0;">
  <img src="_static/wgrib2-logo.png" alt="wgrib2 logo" style="max-width: 550px;">
</div>

!!! warning "Unofficial Documentation"

    This is **unofficial** documentation for wgrib2, scraped and reformatted from the [original CPC documentation](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/){target=_blank}. While I've worked to preserve accuracy, please refer to the official sources for authoritative information and support.

    These are meant to be community docs for wgrib2. **Let's make the best docs for wgrib2 together!** Submit issues or pull requests to [:simple-github: wgrib2-docs](https://github.com/blaylockbk/wgrib2-docs){target=_blank}.

## What is wgrib2?

**wgrib2** is a versatile command-line utility for reading, writing, and manipulating GRIB2 files. Developed by NOAA, it eliminates the need for custom Fortran programs to work with meteorological and oceanographic gridded data.

### Key Capabilities

- **Read and inventory** GRIB2 files with detailed metadata extraction
- **Create subsets** by parameter, level, time, or geographic region
- **Convert formats** to/from IEEE binary, text, CSV, NetCDF, and MySQL
- **Regional extraction** using cookie cutter or map projections
- **High performance** with OpenMP threading and flow-based parallelization
- **Create new fields** and write GRIB2 output

## Official Resources

<div class="grid cards" markdown>

- :material-github: **GitHub Repository**  
  Source code, issues, and releases  
  [NOAA-EMC/wgrib2 →](https://github.com/NOAA-EMC/wgrib2){target=\_blank}

- :material-file-document-outline: **API Documentation**  
  Auto-generated Doxygen docs  
  [View API docs →](https://noaa-emc.github.io/wgrib2/){target=\_blank}

- :material-web: **Original Documentation**  
  CPC wgrib2 documentation  
  [CPC website →](https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/){target=\_blank}

- :material-youtube: **Video Tutorial**  
  EMC wgrib2 seminar presentation  
  [Watch on YouTube →](https://www.youtube.com/watch?v=udbxfC1V2DI){target=\_blank}

</div>

## Quick Start

New to wgrib2? Start with these pages:

- **[Installation](./User-Guide/01_install-conda.md)** - Get wgrib2 running on your system
- **[Basic Usage](./User-Guide/03_best-practices.md)** - Common commands and workflows
- **[Options Reference](options/)** - Complete list of command-line options

!!! info "About This Site"

    This documentation aims to make **wgrib2** more accessible by:

    - ✅ Improving documentation search and navigation using a modern static site generator
    - ✅ Organizing content by topic
    - ✅ Maintaining comprehensive option reference

    I have heavily leveraged ChatGPT to make improvements.
