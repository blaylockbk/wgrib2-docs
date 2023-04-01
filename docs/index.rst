.. raw :: html

   <img src="_static/wgrib2-logo.png" style="background-color:transparent;">


=======================================
The unofficial documentation for wgrib2
=======================================

Wgrib2 is a processor for grib2 files. It is a utility and library for manipulating grib files, The utility was designed to be used to reduce the need for custom Fortran programs to read, write and manipulate grib files.

Wgrib2 has the following abilities.

- inventory and read grib2 files
- create subsets
- create regional subsets by cookie cutter or projections
- export to ieee, text, binary, CSV, netcdf and mysql
- import to ieee, text, binary, and netcdf
- write of new grib2 fields
- parallel processing by using threads (OpenMP)
- parallel processing by flow-based programming

.. toctree::
   :maxdepth: 1

   user_guide/index
   options/index
   pywgrib2_s/index
   release_notes/index

.. attention::

   This is a remake of the original `wgrib2 documentation <https://www.cpc.ncep.noaa.gov/products/wesley/wgrib2/>`_. I scrapped the documentation HTML pages, converted them to Markdown, rebuilt the pages with Sphinx, and hosted them on readthedocs. These, of course, are **unofficial** documentation for wgrib2. I primarily did this because it's sometimes hard to find the right page I'm looking for in the official docs and remade the docs for mainly my own benefit. Plus, this project gave me some experience with Sphinx.

   If you would like to get involved, I would love your help to make *the best wgrib2 documentation ever*. Just start with submitting and Issue or Pull Request in the `wgrib2-docs GitHub repo <https://github.com/blaylockbk/wgrib2-docs>`_.

