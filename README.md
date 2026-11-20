# Diabetes Indicator Prediction 

**Authors:** Alejandro Gutierrez, Barry Siu, Michael Chan, & Migs Dimayacyac

## About

We present a model that classifies whether an individual
has diabetes using only their body mass index (BMI).

The data set that was used in this project ...

## Report

The final report can be found:
***github pages***
(HTML)

## Usage

First time running the project,
install [uv](https://docs.astral.sh/uv/getting-started/installation/)
and [Quarto](https://quarto.org/docs/get-started/),
then clone this repository and move into it:

``` bash
git clone https://github.com/UBC-MDS-2026-27/DSCI-522-group-01-diabetes-indicators-prediction
cd DSCI-522-group-01-diabetes-indicators-prediction
```

Then run the following from the root of this repository
to create the virtual environment:

``` bash
uv sync
```

To run the analysis and render the report,
run the following from the root of this repository:

``` bash
uv run quarto render analysis/diabetes-indicators-prediction.qmd
```

Rendering runs the whole analysis from scratch:
it downloads the data again from the UCI Machine Learning Repository
(so you need an internet connection),
and overwrites the files in `data/raw/` and `data/processed/`.
The rendered report is written to
`analysis/diabetes-indicators-prediction.html`,
replacing the versions in this repository.

## Dependencies

- [uv](https://docs.astral.sh/uv/) (version 0.12.7 or higher)
- [Quarto](https://quarto.org/) (version 1.10.18 or higher)
- Python and packages listed in [`pyproject.toml`](pyproject.toml)
  (exact versions are locked in [`uv.lock`](uv.lock))

## AI use statement

** fill in when analysis report is finished **

## License

This project uses the
[CDC Diabetes Health Indicators](https://archive.ics.uci.edu/dataset/891/cdc+diabetes+health+indicators)
dataset from the UCI Machine Learning Repository
(https://doi.org/10.24432/C53919),
originally derived from the CDC's 2015 Behavioral Risk Factor Surveillance System (BRFSS)
and cleaned by Alex Teboul
([Kaggle](https://www.kaggle.com/datasets/alexteboul/diabetes-health-indicators-dataset)).
The dataset is released under the
[CC0 1.0 Universal (Public Domain)](https://creativecommons.org/publicdomain/zero/1.0/) license.

## References

** Pull references when analaysis report is complete **