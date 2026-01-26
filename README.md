# Training session to set up MESH for the Bow River at Calgary catchment
![Bow River at Calgary Catchment](./0-prerequisites/img/calgary.png)

# Library requirements
## General
Certain libraries and binary executables are necessary to run the
workflows in this repository. Below necessary libraries for general usage
are mentioned:
```console
1. CDO (Climate Data Operators >=v2.2.1),
2. ecCodes (>=v2.25.0),
3. Expat XML parser (>=v2.4.1),
4. GDAL (>=3.5.1),
5. GEOS (>=3.10.2),
6. HDF5 (>=1.10.6),
7. JasPer (>=2.0.16),
8. libaec (>=1.0.6),
9. libfabric (>=1.10.1),
10. libffi (>=3.3),
11. libgeotiff (>=1.7.1),
12. librttopo (>=1.1.0),
13. libspatialindex (>=1.8.5),
14. libspatilite (>=5.0.1),
15. netcdf-fortran (>=4.5.2),
16. netcdf (>=4.7.4),
17. postgresql (>=12.4),
18. proj (>=9.0.1),
19. python (>=3.10.2),
20. sqlite (>=3.38.5),
21. udunits (>=2.2.28)
```
Each of the above libraries and binaries may need further dependencies. It
is up to the user to assure all requirements are satisfied. Most GNU/Linux
distributions should be able to offer all the libraries above through
their remote package repositories. If not, it is recommended to compile
and store them for future reference.

## DRAC Fir HPC
The necessary modules may be loaded with the following command:
```console
ml load libfabric/1.18.0 hdf5/1.14.2 libaec/1.0.6 boost/1.82.0 mpi4py/4.0.3 \
pmix/4.2.4 netcdf/4.9.2 eccodes/2.31.0 eigen/3.4.0 ipykernel/2023b \
ucc/1.2.0 yaxt/0.10.0 netcdf-fortran/4.6.1 arpack-ng/3.9.1 scipy-stack/2023b \
openmpi/4.1.5 fftw/3.3.10 qt/5.15.11 armadillo/12.6.4 code-server/4.101.2 \
gcc/12.3 cdo/2.2.2 geos/3.12.0 cfitsio/4.3.0 calibre/8.6.0 \
r/4.4.0 antlr/2.7.7 librttopo/1.1.0 brunsli/0.1 libreqda/1.0.1 \
rstudio-server/4.4 libdap/3.20.11 freexl/2.0.0 qhull/2020.2 mlflow/3.8.1 \
python/3.11.5 gsl/2.7 libspatialite/5.1.0 lerc/4.0.0 tensorboard/2.20.0 \
ipython-kernel/3.11 nco/5.1.7 libspatialindex/1.9.3 postgresql/16.0 jupyterlab-apps/1.0 \
hwloc/2.9.1 arrow/14.0.1 udunits/2.2.28 libgeotiff/1.7.1 gdal/3.9.1 java/17.0.6 \
ucx/1.14.1 openrefine/3.9.3 jasper/4.0.0 hdf/4.2.16 rust/1.85.0 proj/9.2.0;
```

# Saving Module Collections
It is recommended to save all loaded modules as a "collection" to be able to restore
them whenever needed. You may save them with:
```console
module save scimods # you can change "scimods" to anything!
```

And, you may restore the collection with:
```console
module restore scimods
```
> [!NOTE]
> Please note that these libraries are necessary
for the Python environment to run smoothly (see below).

# Python requirements
## General
The following list of Python packages are required to run much of the
workflows in this repository. The [requirements_fir.txt](./0-prerequisites/requirements_fir.txt)
file describes the packages necessary to run the workflows.

To download this repository on the `$HOME` directory:
```console
git clone https://github.com/kasra-keshavarz/maf-training.git $HOME/github-repos/maf
```

You may create Python virtual environments (after assuring all
the modules are loaded) on HPCs, to isolate the environment
to execute the workflows. On HPCs, typically, it is recommended to use
your `$HOME` directory, so a path like the following is recommended:
```console
python -m venv $HOME/virtual-envs/scienv
```

After creating the virtual environment, you can activate the environment
with:
```console
source $HOME/virtual-envs/scienv/bin/activate
```
And your shell prompt, should look like this:
```console
(scienv) foo@bar: ~$ # this is how your HPC will look
```

After the activation of the virtual environment, you may install any
Python package within the environment. To install those we need for
the modelling workflows:
```console
pip install -r $HOME/github-repos/maf/0-prerequisites/requirements_fir.txt
```

Once the `scienv` is ready, you may add the virtual environment
to the Jupyter Lab as a kernel using the following command:
```console
python -m ipykernel install --name "scienv" --user
```

Once added as a kernel, you should your virtual environment within your
Jupyter sessions.
![Virtual environment within a Jupyter Session](./0-prerequisites/img/jupyter-venv.png)

Last edited: January 26, 2026
