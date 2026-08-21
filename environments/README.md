# Conda environment configs

Each `.yml` file here is a standalone conda environment you can create with:

```bash
conda env create -f environments/<file>.yml
conda activate <env-name>
```

## Available environments

| File | Env name | Purpose |
|---|---|---|
| `geospatial-base.yml` | `geospatial-base` | Base geospatial/ML stack: pandas, numpy, geopandas, rasterio, shapely, pyproj, fiona, xarray/rioxarray, dask, scikit-learn, matplotlib/seaborn/folium, JupyterLab. Good default for most new VMs. |

## Adding a new env file

- Name the file after what it's for, e.g. `deep-learning.yml`, `minimal.yml`.
- Pin `python=` explicitly; leave other packages unpinned unless you have a
  specific reason (reproducibility, a known-bad newer version, etc.) so
  `conda` can resolve a compatible set.
- Prefer the `conda-forge` channel for geospatial packages (GDAL/GEOS/PROJ
  binaries there are the ones the rest of the geo stack expects).
- Update the table above when you add a file.
