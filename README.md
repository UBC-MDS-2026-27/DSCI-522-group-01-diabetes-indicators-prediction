# Diabetes Indicator Prediction 

**Authors:** Alejandro Gutierrez, Barry Siu, Michael Chan, & Migs Dimayacyac

## About

### Summary 
In this project, we investigated whether body mass index (BMI) can be used to predict diabetes status. We used the CDC Diabetes Health Indicators data set, which contains 253,680 survey responses from U.S. adults collected through the 2015 Behavioral Risk Factor Surveillance System, where the target indicates whether a respondent has diabetes or prediabetes. We trained a logistic regression model with BMI as the only predictor and compared it with a dummy classifier that always predicts "no diabetes". Because only a small proportion of respondents have diabetes, we used cross-validation to select a class-balanced model and evaluated performance mainly with the F1 score rather than accuracy. Exploratory analysis showed that respondents with diabetes tend to have higher BMI values. Our model correctly identified more than half of the diabetes cases in the test set, whereas the baseline identified none. However, our model had lower overall accuracy than the baseline because it produced a large number of false positives, and it still missed many true cases. These results suggest that BMI is a useful but limited predictor of diabetes on its own, and that a BMI-only model is not suitable for screening or diagnosis. Adding other known risk factors and validating the model on Canadian data would be needed before such a model could support Canadian public health efforts.

### Data

The data set used in this project is the CDC Diabetes Health Indicators data set, which contains health care statistics and lifestyle survey responses from U.S. adults along with their diabetes status. The data were originally collected by the Centers for Disease Control and Prevention through the Behavioral Risk Factor Surveillance System (BRFSS), an annual health-related telephone survey (Centers for Disease Control and Prevention 2015). The version we use is a cleaned subset of the 2015 BRFSS survey prepared by Alex Teboul (Teboul 2021). It was sourced from the UCI Machine Learning Repository ("CDC Diabetes Health Indicators" 2017) and can be found [here](https://archive.ics.uci.edu/dataset/891/cdc+diabetes+health+indicators).

Each row in the data set represents one survey respondent, giving 253,680 rows and 21 features with no missing values. The target variable, `Diabetes_binary`, indicates whether the respondent has no diabetes (0) or prediabetes or diabetes (1). The features include chronic health conditions (e.g., high blood pressure, high cholesterol, heart disease), lifestyle factors (e.g., BMI, smoking, physical activity, fruit and vegetable consumption), health care access, self-rated physical and mental health, and demographic information (sex, age group, education and income).

(Note: Claude AI helped generate this description of how the data were curated and made publicly available)

## Report

The final report can be found:
<https://ubc-mds-2026-27.github.io/DSCI-522-group-01-diabetes-indicators-prediction/>

## Usage 

Our analysis should run inside a Docker container, ensuring everyone uses the same software and package versions. The image is published on Docker Hub at [Docker dsci-522-group-01-diabetes-indicators-prediction](https://hub.docker.com/r/agutzz/dsci-522-group-01-diabetes-indicators-prediction/)

### Setup

Prior to running this project, install [Docker](https://www.docker.com/get-started/). Every step below uses Docker, so before you run any of the commands, open Docker Desktop and wait until it shows that the Docker engine is running. Docker Desktop needs to stay open the whole time you are running the analysis. Then clone this repository and move into it:

```bash
git clone https://github.com/UBC-MDS-2026-27/DSCI-522-group-01-diabetes-indicators-prediction
cd DSCI-522-group-01-diabetes-indicators-prediction
```

Make sure you run the following commands in the root of this repository.

### 1. Make an ssh key

You will need to make an ssh key if you do not have one yet, run the command below and press enter at each question:

```bash
ssh-keygen -t ed25519
```

Without this ssh key, Docker will automatically make a folder named `~/.ssh/id_ed25519.pub` instead, and connecting to the container over ssh will continually fail until you delete that folder and make an ssh key.

### 2. Download the image:

```bash
docker compose pull
```

### 3. Render the report in the container

```bash
docker compose run --rm analysis-env uv run quarto render
```

Rendering runs the whole analysis from scratch:
it downloads the data again from the UCI Machine Learning Repository (so you need an internet connection), and overwrites the files in `data/raw/` and `data/processed/`. The rendered report is written to `docs/index.html`, replacing the versions in this repository.

This command runs the whole analysis inside a Docker container. Once the Quarto render finishes, the `--rm` flag makes Docker remove the container automatically.

If you see an error such as "Cannot connect to the Docker daemon", Docker Desktop is not running. Open it, wait until the Docker engine is running, and run the command again.

## Contributing

See [`CONTRIBUTING.md`](CONTRIBUTING.md) if you would like to change the analysis, add a package, or update our published report.

## Dependencies

- [Docker](https://www.docker.com/)
- The image is built from the [`Dockerfile`](Dockerfile) within this repository. It will install Quarto, uv, and Python with packages listed in [`pyproject.toml`](pyproject.toml)(exact versions are locked in [`uv.lock`](uv.lock)).


## AI use statement

Generative AI tools were used in parts of this project. Wherever generative AI contributed to the report or code, we added a note at that location stating where it was used and which AI tool generated the content (for example, "Claude AI helped generate this description…" or a code comment noting AI assistance). All AI-generated content was reviewed, edited, and verified by the authors, who take full responsibility for the final report. All contributors followed the UBC Master of Data Science (MDS) policies on the use of generative AI.

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

American Diabetes Association Professional Practice Committee for Diabetes. 2026. "2. Diagnosis and Classification of Diabetes: Standards of Care in Diabetes—2026." *Diabetes Care* 49 (Supplement 1): S27–S49. https://doi.org/10.2337/dc26-S002.

Antar, S. A., N. A. Ashour, M. Sharaky, M. Khattab, N. A. Ashour, R. T. Zaid, E. J. Roh, A. Elkamhawy, and A. A. Al-Karmalawy. 2023. "Diabetes Mellitus: Classification, Mediators, and Complications; A Gate to Identify Potential Targets for the Development of New Effective Treatments." *Biomedicine & Pharmacotherapy* 168: 115734. https://doi.org/10.1016/j.biopha.2023.115734.

"CDC Diabetes Health Indicators." 2017. UCI Machine Learning Repository. https://doi.org/10.24432/C53919.

Centers for Disease Control and Prevention. 2015. "Behavioral Risk Factor Surveillance System Survey Data, 2015." U.S. Department of Health and Human Services, Centers for Disease Control and Prevention. https://www.cdc.gov/brfss/annual_data/annual_2015.html.

Chadha, C., A. G. Pittas, C. W. Lary, W. C. Knowler, R. Chatterjee, L. S. Phillips, V. R. Aroda, et al. 2020. "Reproducibility of a Prediabetes Classification in a Contemporary Population." *Metabolism Open* 6: 100031. https://doi.org/10.1016/j.metop.2020.100031.

Connor Gorber, S., M. Shields, M. S. Tremblay, and I. McDowell. 2008. "The Feasibility of Establishing Correction Factors to Adjust Self-Reported Estimates of Obesity." *Health Reports* 19 (3): 71–82.

Diabetes Canada. n.d. "Diabetes in Canada." Diabetes Canada. https://www.diabetes.ca/what-is-diabetes/diabetes-in-canada.

Ismail, Leila, Huned Materwala, and Juma Al Kaabi. 2021. "Association of Risk Factors with Type 2 Diabetes: A Systematic Review." *Computational and Structural Biotechnology Journal* 19: 1759–1785. https://doi.org/10.1016/j.csbj.2021.03.003.

Lasser, Karen E., David U. Himmelstein, and Steffie Woolhandler. 2006. "Access to Care, Health Status, and Health Disparities in the United States and Canada: Results of a Cross-National Population-Based Survey." *American Journal of Public Health* 96 (7): 1300–1307. https://doi.org/10.2105/AJPH.2004.059402.

Morton, Jedidiah I., Peter A. Lazzarini, Kevan R. Polkinghorne, Bendix Carstensen, Dianna J. Magliano, and Jonathan E. Shaw. 2022. "The Association of Attained Age, Age at Diagnosis, and Duration of Type 2 Diabetes with the Long-Term Risk for Major Diabetes-Related Complications." *Diabetes Research and Clinical Practice* 190: 110022. https://doi.org/10.1016/j.diabres.2022.110022.

Parker, Emily D., Janice Lin, Troy Mahoney, Nwanneamaka Ume, Grace Yang, Robert A. Gabbay, Nuha A. ElSayed, and Raveendhara R. Bannuru. 2024. "Economic Costs of Diabetes in the U.S. in 2022." *Diabetes Care* 47 (1): 26–43. https://doi.org/10.2337/dci23-0085.

Public Health Agency of Canada. 2023. "Snapshot of Diabetes in Canada, 2023." Government of Canada. https://www.canada.ca/en/public-health/services/publications/diseases-conditions/snapshot-diabetes-canada-2023.html.

Public Health Agency of Canada. 2025. "Diabetes in Canada: An Interactive Report on Key Statistics: Overview." Government of Canada. https://health-infobase.canada.ca/diabetes/.

Rafiq, Shamaila, and Per Bendix Jeppesen. 2018. "Body Mass Index, Vitamin D, and Type 2 Diabetes: A Systematic Review and Meta-Analysis." *Nutrients* 10 (9): 1182. https://doi.org/10.3390/nu10091182.

Robinson, Chris A., Gina Agarwal, and Kara Nerenberg. 2011. "Validating the CANRISK Prognostic Model for Assessing Diabetes Risk in Canada's Multi-Ethnic Population." *Chronic Diseases and Injuries in Canada* 32 (1): 19–31.

Statistics Canada. n.d.a. "Health of Canadians: Health Outcomes." Statistics Canada. https://www150.statcan.gc.ca/n1/pub/82-570-x/2024001/section2-eng.htm.

Statistics Canada. n.d.b. "Table 13-10-0394-01: Leading Causes of Death, Total Population, by Age Group." Statistics Canada. https://doi.org/10.25318/1310039401-eng.

Teboul, Alex. 2021. "Diabetes Health Indicators Dataset." Kaggle. https://www.kaggle.com/datasets/alexteboul/diabetes-health-indicators-dataset.

UC Irvine Machine Learning Repository. 2023. "ucimlrepo: Package to Import Datasets from the UC Irvine Machine Learning Repository." Python package version 0.0.7. https://github.com/uci-ml-repo/ucimlrepo.