# Lava flow volume from DEM differencing

This Jupyter notebook estimates mean lava-flow thickness, bulk and dense-rock equivalent volumes, and time-averaged discharge rates from digital elevation models (DEMs). It adapts the lava-volume scripts by Joaquín M. C. Belart and Sydney R. Gunnarson. The main update is the use of [xDEM](https://xdem.readthedocs.io) to coregister the DEMs over stable terrain before differencing. The notebook uses residual elevation changes on stable terrain to estimate uncertainty and writes GeoTIFFs and a text summary to `output/`. [Original `volcano_scripts` workflow](https://github.com/natturufraedistofnun/volcano_scripts/tree/master/volume_scripts)

## Run the example

Keep the notebook, [`requirements.txt`](requirements.txt), and the input files in the same directory. Open a terminal there (Miniconda Prompt on Windows) and run the following commands. [Find your install guide](https://www.anaconda.com/docs/getting-started/installation)

```sh
conda create --name lava-volume --channel conda-forge --override-channels python=3.14 pip
conda activate lava-volume
python -m pip install -r requirements.txt
python -m jupyterlab
```

Open [`lava_volume_calculation.ipynb`](lava_volume_calculation.ipynb), select the Python 3 kernel from the `lava-volume` environment, and run all cells. The example covers the Litli Hrútur eruption on Iceland’s Reykjanes Peninsula from onset at 16:40 UTC on 10 July 2023 to the survey at 18:00 UTC on 11 July. The DEMs have 2 m cells in ISN93 (EPSG:3057), and the notebook uses ISH2004 heights (EPSG:8089). [Icelandic Meteorological Office eruption report](https://en.vedur.is/volcanoes/fagradalsfjall-eruption/news-and-alerts/earthquake-activity-in-fagradalsfjall-area)

### Configure the survey interval

Use the same time zone for `t1` and `t2`. Survey times are usually the midpoint between the start and end of the survey.

- **First calculation.** Set `t1` to eruption onset and `t2` to the first survey midpoint. Set the prior bulk and dense-rock equivalent volumes to zero and leave `new_lava_filename = None`.
- **Second or later survey.** Set `t1` and `t2` to the previous and current survey midpoints. Enter the previous survey’s cumulative bulk and dense-rock equivalent volumes in cubic metres. Set `new_lava_filename` to a polygon layer showing where additional lava was emplaced, including where it overlies earlier flows. The difference between cumulative volumes gives the volume added during the interval. The new-lava area is used to estimate uncertainty in that volume change and its discharge rate.

## Example data and credits

The 11 July 2023 aerial survey was carried out by a collaboration led by Náttúrufræðistofnun. [University of Iceland survey report](https://jardvis.hi.is/is/eldgos-vid-litla-hrut-nidurstodur-maelinga-11-juli)

The pre-eruption DEM, survey DEM, and full mapped lava outline are required for every run. An orthomosaic, if available, can help with tracing the lava outline.

| File | Role |
| --- | --- |
| `20220821_pre-eruption_DEM_2m_zmae.tif` | Reference surface before the 2023 eruption. Original source and reuse terms are pending confirmation. |
| `20230711_1800_DEM_2m_ISN93.tif` | Survey DEM used for differencing. |
| `20230711_1800_lava_outline.gpkg` | Full mapped lava extent used to estimate cumulative volume. |
| `20230711_1800_artifact_mask.gpkg` | Optional polygons marking unreliable parts of the survey DEM to exclude before alignment. Set `artifact_filename = None` to use the survey DEM directly. |
| `20230711_1800_channel_mask.gpkg` | Optional polygons marking channel artifacts to fill by local interpolation after differencing. Set `channel_filename = None` to use the aligned elevation difference directly. |
| `20230711_1800_new_lava.gpkg` | Example polygon layer for repeat surveys, used to estimate uncertainty for the survey interval. |

File-level creator credits and reuse terms for the example GeoPackage layers are pending confirmation.

## Citation and reuse

For background on aerial monitoring and lava-volume estimation, see Pedersen et al. (2022), *Volume, effusion rate, and lava transport during the 2021 Fagradalsfjall eruption: Results from near real-time photogrammetric monitoring*. [Geophysical Research Letters](https://doi.org/10.1029/2021GL097125)

For research use, cite the DOI of the specific Zenodo release of this notebook once available, and cite the source datasets used in the analysis. The notebook and the earlier `volcano_scripts` workflow are licensed under CC BY 4.0. [Notebook license](LICENSE) · [Upstream license](https://github.com/natturufraedistofnun/volcano_scripts/blob/master/LICENSE)
