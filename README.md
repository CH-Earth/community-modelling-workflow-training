# Training session to set up MESH for the Bow River at Calgary catchment
![Bow River at Calgary Catchment](./0-prerequisites/img/calgary.png)

Launching MyBinder Session: [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/kasra-keshavarz/maf-env-basic.git/HEAD)

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
and store them for future reference. In the relevant
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/kasra-keshavarz/maf-env-basic.git/HEAD)
session, these packages are pre-installed and available for testing.

## University of Calgary's ARC HPC
Fortunately, all the above requirements are available on ARC.
You may load the modules with the following command:
```console
# first activating Ucalgary's Computational Hydrology's customized moduled system
. /work/comphyd_lab/local/modules/spack/2024v5/lmod-init-bash
module unuse $MODULEPATH
module use /work/comphyd_lab/local/modules/spack/2024v5/modules/linux-rocky8-x86_64/Core/
# activating modules needed for this workshop
module load \
  gcc/14.2.0 htop/3.3.0 glibc/2.28 libaec/1.0.6 \
  gcc-runtime/14.2.0 hdf5/1.14.3 openssl/3.3.1 lz4/1.9.4 \
  libevent/2.1.12 snappy/1.1.10 numactl/2.0.14 c-blosc/1.21.5 \
  opa-psm2/11.2.230 netcdf-c/4.9.2 krb5/1.21.2 \
  netcdf-fortran/4.6.1 libedit/3.1-20230828 openjpeg/2.3.1 \
  libxcrypt/4.4.35 eccodes/2.34.0 openssh/9.8p1 \
  fftw/3.3.10 ucx/1.17.0 libunistring/1.2 \
  openmpi/4.1.6 libidn2/2.3.7 sqlite/3.46.0 \
  nghttp2/1.62.0 libmd/1.0.4 curl/8.7.1 \
  libbsd/0.12.2 libjpeg-turbo/3.0.3 expat/2.6.2 \
  libtiff/4.6.0 libffi/3.4.6 proj/9.4.1 \
  util-linux-uuid/2.40.2 cdo/2.4.3 python/3.11.7 \
  antlr/2.7.7 python-venv/1.0 gsl/2.7.1 \
  py-numpy/1.26.4 nco/5.2.4 gdal/3.9.2 \
  tree/2.1.0 geos/3.12.2 which/2.21 \
  udunits/2.2.28 r/4.4.1 slurm/24.11.0-1 \
  py-mpi4py/4.0.0 qt/5.15.14;
```

It is recommended to save all load modules as a list to be able to restore
them whenever needed. Using the LMOD features, you may save them with:
```console
module save scimods # you can change "scimods" to anything!
```

And, you may restore the list with:
```console
module restore scimods
```
> [!NOTE]
> Please note that these libraries are necessary
for the Python environment to run smoothly (see below).

# Python requirements
## General
The following list of Python packages are required to run much of the
workflows in this repository. The [requirements.txt](./0-prerequisites/requirements.txt)
file describes the packages necessary to run the workflows.

To download this repository on the `$HOME` directory:
```console
git clone https://github.com/kasra-keshavarz/community-modelling-workflow-training.git $HOME/github-repos/community-workflows
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
pip install -r $HOME/github-repos/community-workflows/0-prerequisites/requirements.txt
```

Once the `scienv` is ready, you may add the virtual environment
to the Jupyter Lab as a kernel using the following command:
```console
python -m ipykernel install --name "scienv" --user
```

Once added as a kernel, you should your virtual environment within your
Jupyter sessions.
![Virtual environment within a Jupyter Session](./0-prerequisites/img/jupyter-venv.png)

Last edited: February 17th, 2025
