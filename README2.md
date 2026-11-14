# walmart-hurricane-project

DS2002 (Fall 2026) midterm project: Walmart sales around Hurricane Helene, September 2024. The notebook cleans the supplied sales, store, product and inventory data, joins it to hourly weather from the Open-Meteo API, and answers the five questions in the project brief.

Team: Viet ([@viet-huynh-1](https://github.com/viet-huynh-1)), solo. See [CHARTER.md](CHARTER.md).

## Contents

- `notebooks/walmart_hurricane_analysis.ipynb`: the analysis
- `notebooks/midterm_checkpoint.ipynb`: the week 1 checkpoint
- `data/`: the supplied data files, plus a saved copy of the weather API response
- `REFLECTION.md`: answers to the reflection questions

## How to run

Requires Python 3.10 or newer. From the project folder:

```
python -m venv .venv
.venv\Scripts\activate          (Windows)
source .venv/bin/activate       (macOS/Linux)
python -m pip install pandas numpy matplotlib requests notebook
python -m notebook notebooks/walmart_hurricane_analysis.ipynb
```

Then run all cells (Kernel > Restart & Run All). The weather cell needs an internet connection. If the API can't be reached, the notebook uses the saved response in `data/weather_cache/` and says so in its output. Running the notebook also creates `figures/` and `output/walmart_clean.db`.

## Data sources

- Course data from the DS2002 repository, `notebooks/06-midterm-walmart/data`
- Weather: Open-Meteo Historical Weather API, `https://archive-api.open-meteo.com/v1/archive`
- Landfall time and location: NHC Tropical Cyclone Update for Hurricane Helene, Sept 26, 2024
