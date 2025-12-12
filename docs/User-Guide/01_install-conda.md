# Install from conda-forge

The easiest way to install _wgrib2_ for Linux or Mac is from **conda-forge**.

[![Conda-version](https://img.shields.io/conda/v/conda-forge/wgrib2)](https://anaconda.org/conda-forge/wgrib2)

=== ":simple-hackthebox: pixi"

    Install wgrib2 globally using [pixi](https://pixi.sh/latest/)

    ```bash
    pixi global install wgrib2
    ```

    ```console
    $ pixi  global install wgrib2
    └── wgrib2: 3.8.0 (installed)
        └─ exposes: gmerge, smallest_4, smallest_grib2, wgrib, wgrib2
    ```

=== ":simple-condaforge: Mamba"

    Create a conda environment named "wgrib2" with the `wgrib` package installed.

    ```bash
    mamba create -n wgrib2 wgrib2
    ```

    Now you can run the wgrib2 command with

    ```bash
    mamba run -n wgrib2 wgrib2 <options>
    ```

    Or activate the environment and then run:

    ```bash
    mamba activate wgrib2
    wgrib2 <options>
    ```

=== ":simple-anaconda: Conda"

    Install wgrib2 from conda-forge:

    ```bash
    conda install conda-forge::wgrib2
    ```

!!! note "There are no pre-built wgrib2 builds for Windows."
