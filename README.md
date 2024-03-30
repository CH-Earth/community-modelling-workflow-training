# Training session to set up MESH for the Bow River at Banff catchment
![Bow River at Banff Catchment](./0-prerequisites/img/bow.png)

To download this repository on the `$HOME` directory of your Graham account:
```console
foo@gra-login1:~$ git clone https://github.com/kasra-keshavarz/community-modelling-workflow-training.git ./github-repos/community-workflows
```


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

## Digital Research Alliance of Canada (DRA) Graham HPC
Fortunately, all the above requirements are available on the DRA's Graham
HPC. You may load the modules with the following command:
```console
foo@bar:~$ module load StdEnv/2020
foo@bar:~$ module load gcc/9.3.0
foo@bar:~$ module load \
  sqlite/3.38.5 postgresql/12.4 gdal/3.5.1 \
  udunits/2.2.28 cdo/2.2.1 gentoo/2020 \
  imkl/2020.1.217 openmpi/4.0.3 libfabric/1.10.1 \
  jasper/2.0.16 freexl/1.0.5 geos/3.10.2 \
  libaec/1.0.6 mpi4py/3.1.3 \
  libffi/3.3 hdf5/1.10.6 \
  libgeotiff-proj901/1.7.1 librttopo-proj9/1.1.0 \
  proj/9.0.1 eccodes/2.25.0 netcdf-fortran/4.5.2 \
  mii/1.1.2 ucx/1.8.0 python/3.10.2 \
  netcdf/4.7.4 cfitsio/4.1.0 \
  libspatialite-proj901/5.0.1 expat/2.4.1 \
  yaxt/0.9.0 libspatialindex/1.8.5 arrow/13.0.0 \
  scipy-stack/2023b ipykernel/2023b;
```

> [!NOTE]
> Both `scipy-stack/2023b` and `ipykernel/2023b` need to be loaded at the
> end to assure the `sys.path` addresses in Python sessions are ordered as
> expected.


It is recommended to save all load modules as a list to be able to restore
them whenever needed. Using the LMOD features, you may save them with:
```console
foo@bar:~$ module save scimods # you can change "scimods" to anything!
```

And, you may restore the list with:
```console
foo@bar:~$ module restore scimods
```
> [!NOTE]
> Please note that some of the libraries and binary programs are necessary
for the Python environment to run smoothly (see below).

# Python requirements
## General
The following list of Python packages are required to run much of the
workflows in this repository. The [requirements.txt](./0-prerequisites/requirements.txt)
file describes the packages necessary to run the workflows.

Please refer to [DRA's
manual](https://docs.alliancecan.ca/wiki/Python#Creating_and_using_a_virtual_environment)
for necessary information on how to create a Python virtual environment
using the [requirements.txt](./0-prerequisites/requirements.txt) file mentioned above.

The installation process needs to be done in the login node of the Graham
cluster, so let's switch to a login node:
```console
foo@bar:~$ ssh gra-login1 # user your username and password
```

Once you login, your sheel will look like the following:
```console
foo@gra-login1:~$ 
```

Whenever you change a node, make sure you load all the necessary modules:
```console
foo@gra-login1:~$ module restore scimods
```

Then, you may create Python virtual environments (after assuring all
the modules are loaded) on Graham HPC, to isolate the environment
to execute the workflows. on Graham, it is recommended to use
your `$HOME` directory, so a path like the following is recommended:
```console
foo@gra-login1:~$ python -m virtualenv $HOME/virtual-envs/scienv
```

After creating the virtual environment, you can activate the environment
with:
```console
foo@gra-login1:~$ source $HOME/virtual-envs/scienv/bin/activate
(scienv) foo@gra-login1:~$ # this is how your Graham sheel will look
```

After the activation of the virtual environment, you may install any
Python package within the environment. To install those we need for
the modelling workflows:
```console
(scienv) foo@gra-login1:~$ pip install -r ~/github-repos/community-workflows/0-prerequisites/requirements.txt
```

Once the `scienv` is ready, you may add the virtual environment
to the Jupyter Lab as a kernel using the following command:
```console
(scienv) foo@gra-login1:~$ python -m ipykernel install --name "scienv" --user
```
> [!IMPORTANT]
> If you face any errors by executing the command above, make sure
> `jupyter` and `ipykernel` packages are installed properly. Similarly,
> you may again use `pip` to install these packages.

Once added as a kernel, you should your virtual environment within your
Jupyter sessions.
![Virtual environment within a Jupyter Session](./0-prerequisites/img/jupyter-venv.png)

# Additional datasets necessary
1. MERIT-Basins vector hydrography Dataset (v0.7/v1.0, minor bug fix for coastaline pixels): https://www.reachhydro.org/home/params/merit-basins </b>

   `MERIT-Basins` is available on Graham HPC under the following directory:
   ```console
   /project/rrg-mclark/data/geospatial-data/MERIT-Basins # rpp-kshook (GWFO) allocation
   /project/rpp-kshook/Climate_Forcing_Data/geospatial-data/MERIT-Basins # rrg-mclark allocation
   ```

2. Datatool (version v0.5.1-dev): https://github.com/kasra-keshavarz/datatool </b>

   Download with:
   ```console
   foo@gra-login1:~$ git clone https://github.com/kasra-keshavarz/datatool.git ./github-repos/datatool
   ```

3. GIStool (version v0.1.7-dev, commit ff2a6da): https://github.com/kasra-keshavarz/gistool </b>

   Download with:
   ```console
   foo@gra-login1:~$ git clone https://github.com/kasra-keshavarz/gistool.git ./github-repos/gistool
   ```

4. EASYMORE (v2.0.0-dev): https://github.com/ShervanGharari/EASYMORE </b>
  
   Download with:
   ```console
   foo@gra-login1:~$ pip install git+https://github.com/ShervanGharari/EASYMORE.git ./github-repos/easymore
   ```

Last edited: March 30th, 2024
