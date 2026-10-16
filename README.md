# NBA Player Performance Analysis

## Project Overview

This project analyzes NBA player performance and team success using
historical NBA data.

The goal is to build an analysis-ready dataset by collecting, cleaning,
correcting, and combining multiple NBA data sources, followed by
descriptive analysis, regression models, and classification models.

## Project Structure

    NBA_Project/

    ├── code/
    │   ├── data preparation and cleaning/
    │   └── analysis/
    │       ├── Descriptive/
    │       ├── Regression/
    │       └── Classification/

    ├── data/
    │   ├── raw/
    │   └── processed/

    └── README.md
# Code Structure

All notebooks and analysis workflows are located inside the `code/` folder.

The folder is divided into two main parts:

- Data Preparation and Cleaning
- Analysis

---

## Data Preparation and Cleaning

Location:

`code/data preparation and cleaning/`

The data preparation pipeline combines multiple NBA datasets into a unified analysis dataset.

Main steps:

- Collected raw player performance, biography, team standings, and playoff information.
- Cleaned and standardized variables.
- Corrected inconsistent player attributes such as height, weight, and draft information.
- Added career-related information.
- Integrated team and playoff information.
- Created the final player-season analysis dataset.

Detailed documentation is available in:

`code/data preparation and cleaning/README.md`

---

## Analysis

Location:

`code/analysis/`

The analysis folder contains all descriptive and predictive modeling workflows.

### Descriptive Analysis

Investigates:

- Dataset structure
- Missing values
- Player and team characteristics
- Season-level consistency
- Possible player-team attribution issues

### Regression Analysis

Predicts player performance using:

- Ordinary Least Squares Regression
- Generalized Additive Models (GAM)
- Random Forest Regression

Includes:

- Feature selection and analysis
- Model evaluation
- Feature importance analysis

### Classification Analysis

Predicts playoff qualification using:

- Logistic Regression
- Lasso Logistic Regression
- Ridge Logistic Regression
- Random Forest Classification

Includes:

- Model comparison
- ROC-AUC evaluation
- Feature importance analysis
## Data Organization

The `data` folder contains all datasets used throughout the project and is divided into two main parts:

### Raw Data

The `raw` folder contains the original datasets collected from the different NBA sources before any cleaning or modification.

Included datasets:

- Player performance statistics
- Player biography information
- Player information data
- Team standings
- Playoff series history

These files represent the initial data sources used for the cleaning pipeline.

### Processed Data

The `processed` folder contains the cleaned and standardized datasets after applying:

- Data cleaning
- Variable selection
- Format standardization
- Manual corrections
- Feature preparation

The main output is:

`nba_final_player_season_data`

which combines player, career, team, and playoff information and is used for all the analyses.
## Data Sources

The project uses NBA data from:

-   NBA API
-   Basketball Reference

The datasets include:

-   Player statistics
-   Player biography information
-   Career information
-   Team standings
-   Playoff history

## Tools and Libraries

-   Python
-   pandas
-   numpy
-   scikit-learn
-   pygam
-   statsmodels
-   matplotlib
-   nba_api
-   pathlib
-   difflib
-   time
-   warning
