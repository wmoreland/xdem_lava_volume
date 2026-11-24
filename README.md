# Lava flow volume from DEM differencing

This Jupyter notebook estimates mean lava-flow thickness, bulk and dense-rock equivalent volumes, and time-averaged discharge rates from digital elevation models (DEMs). It adapts the lava-volume scripts by Joaquín M. C. Belart and Sydney R. Gunnarson. The main update is the use of xDEM to coregister the DEMs over stable terrain before differencing. The notebook uses residual elevation changes on stable terrain to estimate uncertainty and writes GeoTIFFs and a text summary to `output/`. [Original `volcano_scripts` workflow](https://github.com/natturufraedistofnun/volcano_scripts/tree/master/volume_scripts)

## Run the example

Keep the notebook, [`requirements.txt`](requirements.txt), and the input files in the same directory. Open a terminal there (Miniconda Prompt on Windows) and run the following commands. [Find your install guide](https://www.anaconda.com/docs/getting-started/installation)

```sh
conda create --name lava-volume --channel conda-forge --override-channels python=3.14 pip
conda activate lava-volume
python -m pip install -r requirements.txt
python -m jupyterlab
```

Open [`lava_volume_calculation.ipynb`](lava_volume_calculation.ipynb) and run all cells. The example uses a pre-eruption reference DEM and the 11 July 2023 survey of the Litli Hrútur eruption on Iceland’s Reykjanes Peninsula. The DEMs have 2 m cells in ISN93 (EPSG:3057), and the notebook uses ISH2004 heights (EPSG:8089).

For the first calculation, use eruption onset and the first survey midpoint. For later calculations, use the previous and current survey midpoints, the previous cumulative volumes, and the polygon of lava emplaced between the surveys. The polygon’s area is used to estimate uncertainty in the change in volume and the discharge rate.

## Example data and credits

The 11 July 2023 aerial survey was carried out by a collaboration lead by Náttúrufræðistofnun. [University of Iceland survey report](https://jardvis.hi.is/is/eldgos-vid-litla-hrut-nidurstodur-maelinga-11-juli)

| File | Role |
| --- | --- |
| `20220821_pre-eruption_DEM_2m_zmae.tif` | Reference surface before the 2023 eruption. Original source and reuse terms are pending confirmation. |
| `20230711_1800_DEM_2m_ISN93.tif` | Survey DEM used for differencing. |
| `20230711_1800_lava_outline.gpkg` | Full mapped lava extent used to estimate cumulative volume. |
| `20230711_1800_artifact_mask.gpkg` | Polygons marking reconstruction artifacts excluded from the survey DEM. |
| `20230711_1800_channel_mask.gpkg` | Polygons marking channel artifacts treated by local interpolation. |
| `2023-07-11_1800_new_lava.gpkg` | Optional polygon of lava emplaced between surveys, used when estimating uncertainty for that interval. |
| `20230711_1800_ORTHO_50cm_ISN93.tif` | Optional 50 cm orthomosaic for visual context. |

File-level creator credits and reuse terms for the example GeoPackage layers are pending confirmation.

## Citation and reuse

For research use, cite the DOI of the specific Zenodo release of this notebook once available, and cite the source datasets used in the analysis. The earlier `volcano_scripts` workflow is licensed under CC BY 4.0. [Upstream license](https://github.com/natturufraedistofnun/volcano_scripts/blob/master/LICENSE)
