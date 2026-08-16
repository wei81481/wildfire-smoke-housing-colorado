# Wildfire Smoke and Housing Prices in Colorado

This repository contains the Python code used to study the relationship between wildfire-attributable smoke PM2.5 exposure and residential closing prices in Colorado from 2018 to 2022.

The analysis links property transactions to daily smoke exposure on a 10 km grid, constructs distributed smoke-lag measures, adds school, environmental, and COVID-19 controls, and estimates fixed-effects hedonic models with alternative clustered standard errors.

This repository is provided for analytical transparency and conditional reproduction. It is not a self-contained reproduction package because the underlying CoreLogic housing transaction records were accessed under a third-party data-use agreement and cannot be redistributed by the authors. Complete reproduction therefore requires independently authorized access to the CoreLogic data and the additional public or derived inputs documented below.

## Main analysis file

- [`Revision/Smoke-Housing.py`](Revision/Smoke-Housing.py): data preparation, smoke imputation and lag construction, data merging, variable construction, and primary model estimation.

The file is organized as a sequential research workflow. Run its sections in order from the repository root after placing the required input files in the locations listed below.

## Analysis workflow

The main script performs the following steps:

1. Cleans Colorado residential transaction records and removes rental listings.
2. Identifies and matches K–12 school names to annual Colorado school-performance scores.
3. Adjusts closing prices to 2010 statewide HPI terms and constructs housing controls.
4. Assigns transactions to 10 km grid cells.
5. Combines wildfire-smoke estimates with fire-distance information and imputes missing winter smoke values using XGBoost.
6. Constructs eight three-month smoke-exposure windows covering the 0–24 months before closing.
7. Adds MERRA-2 environmental controls, COVID-19 measures, pandemic-phase indicators, and a ski-county indicator.
8. Estimates the fixed-effects hedonic model and calculates HC1, grid-clustered, and two-way clustered standard errors.

## Software requirements

- Python 3.10 or later
- A Python environment capable of installing GeoPandas and its spatial dependencies

Install the required Python packages with:

```bash
python -m venv .venv
source .venv/bin/activate       # macOS or Linux
# .venv\Scripts\activate        # Windows

python -m pip install --upgrade pip
python -m pip install numpy pandas geopandas shapely matplotlib seaborn \
    statsmodels scikit-learn xgboost pyreadr openpyxl
```

## Required input data

The code expects the following files or folders. File names can be changed, but the corresponding paths in the script must also be updated.

| Input | Expected location | Availability |
|---|---|---|
| CoreLogic transaction records | `CO_all_mls_2008_2024.csv` | Restricted third-party data; not included |
| Daily wildfire-attributable smoke PM2.5 | `smokePM2pt5_predictions_daily_10km_20060101-20231231.rds` | Obtain from the original data provider |
| Fire-distance data | `combined_distance_to_fire.csv` | Derived input; not included unless separately deposited |
| Colorado 10 km grid | `10km_grid_wgs84/` | Spatial input |
| Colorado state boundary | `Colorado_State_Boundary/` | Public spatial input |
| Colorado county boundaries | `Colorado_County_Boundaries/` | Public spatial input |
| Colorado school codes | `Codes_2024-2025_CO-Schools.xlsx` | Colorado Department of Education |
| School performance, 2018–2021 | `SPFFinalRatings2010to2021_0.xlsx` | Colorado Department of Education |
| School performance, 2022 | `SPF2022_FinalRatingsOverTime_120522.xlsx` | Colorado Department of Education |
| FHFA House Price Index | `hpi_master.csv` | Federal Housing Finance Agency |
| Environmental controls | `colorado_env_controls_2018_2022.csv` | Derived from NASA MERRA-2 |
| County COVID-19 cases | `covid_confirmed_usafacts.csv` | USAFacts historical data |
| Smoke data with lag/lead terms | `daily_smoke_imputed_with_lagsleads.csv` | Derived intermediate file |

Large, restricted, licensed, or derived data files should not be committed to this repository unless their licences explicitly permit redistribution.

## Running the analysis

Clone the repository and enter its root directory:

```bash
git clone https://github.com/YOUR-USERNAME/wildfire-smoke-housing-colorado.git
cd wildfire-smoke-housing-colorado
```

Create the environment and install the dependencies as described above. Place the input data in the expected locations, then run:

```bash
python Revision/Smoke-Housing.py
```

Because the script follows the original research workflow, it may be more convenient to run it section by section in an interactive Python environment such as VS Code, Spyder, or Jupyter. All paths are interpreted relative to the repository root. The command above will not reproduce the manuscript results unless the user has authorized access to the restricted CoreLogic records and has prepared all required public and derived inputs.

## Reproducibility and data restrictions

- The authors cannot provide or authorize access to the CoreLogic transaction records. Researchers must obtain access independently from the data provider and comply with the applicable licence and data-use agreement.
- Researchers with authorized access may adapt the code using the variable names, processing steps, and analytical workflow documented in the script and accompanying materials.
- Public source data must be obtained from their respective providers and remain subject to the providers' original terms of use.
- Derived smoke-lag and environmental-control files must be generated from the corresponding preparation steps or obtained from an accompanying research archive if one is provided.
- Random seeds are specified for the XGBoost cross-validation where applicable, but exact reproduction may also depend on software versions and the versions of the input datasets.
- The software licence for this repository applies only to the original source code. It does not grant permission to access, reproduce, or redistribute CoreLogic records or other third-party datasets.
- API keys, credentials, personal file paths, restricted transaction records, and large generated outputs must not be committed to the repository.

## Suggested repository structure

```text
wildfire-smoke-housing-colorado/
├── README.md
├── Revision/
│   └── Smoke-Housing.py
├── data/
│   └── README.md              # data sources and access instructions only
├── outputs/                   # generated tables and figures; optional
└── .gitignore
```

The current script uses the original file locations shown above. If inputs are moved into `data/`, update the paths in the script accordingly.

## Data and code availability

The Python code used to construct the smoke-exposure measures, prepare the analytical variables, estimate the reported models, and generate the corresponding outputs is provided in this repository. The underlying CoreLogic housing transactions are restricted third-party data accessed under a data-use agreement and cannot be redistributed by the authors. The repository is therefore not a self-contained reproduction package. Researchers who independently obtain authorized access to the required data may use the documented variable schema and processing workflow to reproduce or extend the analysis. Public source data should be obtained from their respective providers.

## Citation

If you use this code, please cite the associated article:

> Wei, S., Chen, Y., Zhou, S., et al. *Housing Prices Decline with Wildfire Smoke Exposure across Space and Time in Colorado*. Manuscript under review.

Update this citation with the final journal, year, DOI, and repository release information after publication.

## License

The authors intend to release the original source code under the MIT License. A `LICENSE` file should be added before the repository is made public or submitted as the final archived code release. The MIT License will apply only to the source code authored for this study. It will not grant any right to access, reproduce, redistribute, or reuse CoreLogic transaction records or other third-party datasets, which remain subject to their original licences, data-use agreements, and access restrictions.

## Contact

For questions about the code, open a GitHub issue or contact the corresponding authors listed in the associated manuscript.
