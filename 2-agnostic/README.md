# Introduction
This directory automates the "model-agnostic" part of setting up
hydrological models, specifically, running `datatool`, `gistool`, and
`easymore` to extract necessary information to set up various hydrological
models. Below the detail of each workflow is explained:

Modify any segment of the `JSON` file as needed. Get familiar with 
the various tools downloaded and called with this tool first. Visit
the link of each tool!


# Workflows
1. datatool (version v0.5.1-dev):
  https://www.github.com/kasra-keshavarz/datatool

  This workflow simply prepares meteorological datasets by subsetting
  geographical and temporal extents. 

2. gistool (version v0.1.7-dev):
  https://www.github.com/kasra-keshavarz/gistool

  This workflow simply prepares geospatial datasets, such as landcover
  and soil maps, for hydrological modelling purposes. Preparation is
  done by geographical (and if applicable, temporal) subsetting of the
  original datasets, as well as calculating zonal statistics for the
  geofabrics of interest.

3. easymore (version v2.0.0-dev):
  https://github.com/ShervanGharari/EASYMORE

  This workflow calculates aerial average of meteorological datasets
  (in this setup, using the outputs of datatool) for computational
  elements of hydrological models. In the current setup, sub-basins
  are the targets.

4. model-agnostic.sh (version v0.1.0-dev0):
  https://github.com/kasra-keshavarz/agnostic-orchestrator

  Workflow to execute all mentioned workflows above in a hierarchical
  manner to minimize user interaction with the workflows themself.

5. model-agnostic.json (version v0.1.0-dev0):
  https://github.com/kasra-keshavarz/agnostic-orchestrator

  Global configuration file to execute model-agnostic workflows on
  Digital Research Alliance of Canada (DRA)'s Graham HPC in an attempt
  to minimize user interactions with the workflows mentioned above.

  The run the "agnostic orchestrator", you need to simply provide the
  input JSON file to the Bash script. Please make sure all the necessary
  modules and Python environment are loaded beforehand:
  ```console
  (scienv) foo@gra-login1: 2-agnostic$ ./model-agnostic.sh model-agnostic.json
  ```

# Steps
## `datatool` for CaSRv3.2
```console
$HOME/github-repos/datatool/extract-dataset.sh --dataset=CaSRv3.2 \
  --dataset-dir=/project/rrg-alpie/data/meteorological-data/casrv3.2/ \
  --variable=CaSR_v3.2_P_P0_SFC,CaSR_v3.2_P_TT_09975,CaSR_v3.2_P_UVC_09975,CaSR_v3.2_A_PR0_SFC,CaSR_v3.2_P_FB_SFC,CaSR_v3.2_P_FI_SFC,CaSR_v3.2_P_HU_09975 \
  --output-dir=$HOME/scratch/bc-models/datatool-outputs \
  --start-date=1980-01-01T13:00:00 \
  --end-date=1980-01-5T12:00:00 \
  --shape-file=$HOME/github-repos/maf/1-geofabric/bow-at-calgary-geofabric/bcalgary_subbasins.shp \
  --cache=$SCRATCH/cache/ \
  --prefix=bc_model_ \
  --cluster=$HOME/github-repos/datatool/etc/clusters/drac.json;
```

## `gistool` for Landcover
```console
$HOME/github-repos/gistool/extract-gis.sh --dataset=landsat \
  --dataset-dir=/project/rrg-alpie/data/geospatial-data/Landsat \
  --variable=land-cover \
  --start-date=2015 \
  --end-date=2015 \
  --output-dir=$HOME/scratch/bc-models/gistool-outputs \
  --shape-file=$HOME/github-repos/maf/1-geofabric/bow-at-calgary-geofabric/bcalgary_subbasins.shp \
  --print-geotiff=true \
  --stat=frac,coords \
  --cache=$SCRATCH/cache/ \
  --prefix=bc_model_ \
  --fid=COMID \
  --cluster=$HOME/github-repos/gistool/etc/clusters/darc.json \
  --include-na;
```

## `easymore` for meteorological data remapping
```console
easymore cli --case-name=remapped \
  --cache=$SCRATCH/cache/ \
  --shapefile=$HOME/github-repos/maf/1-geofabric/bow-at-calgary-geofabric/bcalgary_subbasins.shp \
  --shapefile-id=COMID \
  --source-nc=$HOME/scratch/bc-models/datatool-outputs/**/*.nc* \
  --variable-lon=lon \
  --variable-lat=lat \
  --variable=CaSR_v3.2_P_P0_SFC,CaSR_v3.2_P_TT_09975,CaSR_v3.2_P_UVC_09975,CaSR_v3.2_A_PR0_SFC,CaSR_v3.2_P_FB_SFC,CaSR_v3.2_P_FI_SFC,CaSR_v3.2_P_HU_09975 \
  --remapped-var-id=COMID \
  --remapped-dim-id=COMID \
  --output-dir=$HOME/scratch/bc-models/easymore-outputs/
```