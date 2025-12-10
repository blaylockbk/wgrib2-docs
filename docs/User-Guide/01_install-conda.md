# Install with Conda

The easiest way to install **wgrib2** for linux is with conda.

[![Conda-version](https://img.shields.io/conda/v/conda-forge/wgrib2)
![Conda-platform](https://img.shields.io/conda/pn/conda-forge/wgrib2)](https://anaconda.org/conda-forge/wgrib2)

=== ":simple-condaforge: Mamba"

    Create a conda environment named "wgrib2" with the `wgrib` package installed.

    ```bash
    mamba create -n wgrib2 wgrib2
    ```

    Now you can run the wgrib2 command with

    ```bash
    mamba run -n wgrib2 wgrib2 <options>
    ```

    or activate the environment and then run

    ```bash
    mamba activate wgrib2
    wgrib2 <options>
    ```

=== ":simple-anaconda: Conda"

    Install wgrib2 from the conda-forge channel:

    ```bash
    conda install conda-forge::wgrib2
    ```
