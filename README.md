# Glacier retreat and glacial lake growth in the Everest massif (Sentinel-2, 2017–2025)

Remote Sensing & Computer Vision, Lab 2 (part 2). Authors: Luka Rondal & Tanguy Mandrillon.

The notebook [Lab2_Everest_Glacier.ipynb](Lab2_Everest_Glacier.ipynb) downloads one post-monsoon Sentinel-2 L2A image per year for two areas, **Khumbu Glacier** and **Imja Tsho**. It then:

1. masks clouds and missing data with the Scene Classification Layer (SCL),
2. computes the NDSI and NDWI indices,
3. classifies each pixel as water, snow/ice, rock or cloud/no data,
4. computes the snow/ice area and the lake area for each year, with a linear trend and a change map,
5. validates the results against the literature.

## Requirements

- [uv](https://docs.astral.sh/uv/getting-started/installation/) (it installs the right Python version for you; the project uses Python 3.14)
- A free [Copernicus Data Space Ecosystem](https://dataspace.copernicus.eu/) account with an **OAuth client** (Sentinel Hub API)

All Python dependencies are declared in [pyproject.toml](pyproject.toml) and pinned in [uv.lock](uv.lock).

## Installation

```bash
git clone https://github.com/lrondal/sentinel2-everest-analysis.git
cd sentinel2-everest-analysis
uv sync
```

`uv sync` creates the virtual environment in `.venv/` and installs every dependency, including the Jupyter kernel.

## Copernicus credentials

The credentials are never written in the notebook. They are read from a local file, `myCopernicusCredentials.py`, which is ignored by git.

1. Log in to the [Copernicus Data Space dashboard](https://shapps.dataspace.copernicus.eu/dashboard/#/). Under **User settings → OAuth clients**, create a new client and copy its **client ID** and **client secret**. The secret is shown only once.
2. Copy the example file:

   ```bash
   cp myCopernicusCredentials_Example.py myCopernicusCredentials.py
   ```

3. Fill in `client_id` and `client_secret` in `myCopernicusCredentials.py`. The notebook only uses these two fields.

## Running the notebook

**VS Code:** open `Lab2_Everest_Glacier.ipynb`, select the kernel `.venv` (Python 3.14), then click **Run All**.

**Jupyter Lab:**

```bash
uv run --with jupyterlab jupyter lab Lab2_Everest_Glacier.ipynb
```

**Headless run** (useful to check that everything works; the result is written to a separate file):

```bash
uv run --with nbconvert jupyter nbconvert --to notebook --execute Lab2_Everest_Glacier.ipynb --output executed.ipynb
```

The first run downloads 2 areas × 9 years of images and takes a few minutes. The downloads are cached in `data/sentinelhub/`, so later runs reload them from disk. Delete this folder to download everything again.

## Changing the parameters

All the variable inputs are in the **Parameters** cell (section 2): the bounding boxes, the years, the season window, the cloud limits, the resolution, the NDSI/NDWI thresholds, the lake location, the years excluded from the trends and the years of the change map. The markdown table above that cell explains each value. Change the cell, then re-run the whole notebook.

Bounding boxes can be drawn with [bboxfinder.com](http://bboxfinder.com/) (WGS84, `lon_min, lat_min, lon_max, lat_max`). Keep each area under 2500 × 2500 px at the chosen resolution; the notebook prints a warning otherwise.

## Project structure

| File | Content |
|:---|:---|
| `Lab2_Everest_Glacier.ipynb` | The whole pipeline: parameters, acquisition, pre-processing, classification, change detection, visualisation, validation |
| `myCopernicusCredentials_Example.py` | Template for the credentials file |
| `pyproject.toml` / `uv.lock` | Dependencies managed by uv |
| `data/` | Downloaded Sentinel Hub images (created on first run, not versioned) |
