# Analysis

This folder contains the statistical analysis and machine learning models performed on the final NBA dataset.

The analysis is based on the cleaned and merged dataset created during the data preparation phase:

`nba_final_player_season_data.csv`

The analyses include exploratory data analysis, regression models, and classification models.
## Folder Structure

    code/
    │
    ├── analysis/
        │
        ├── Descriptive/
        │   └── Descriptive.ipynb
        │   
        │
        ├── Regression/
        │   ├── Regression analysis.ipynb
        │   ├── GAM Important Predictors.pdf
        │   ├── MTRY Tuning.pdf
        │   ├── predictor_screening.pdf
        │   ├── Random Forest Max Features Tuning.pdf
        │   └── Random Forest Permutation Importance.pdf
        │
        └── Classification/
            ├── Classification.ipynb
            ├── lasso_regularization_path.pdf
            ├── ridge_regularization_path.pdf
            ├── MTRY Tuning Class.pdf
            ├── Offensive Predictors Playoff Qualification.pdf
            └── ROC Curves.pdf

# 1. Descriptive Analysis

Notebook:

`Descriptive.ipynb`

This notebook performs exploratory analysis of the final NBA dataset.

## Main steps

- Loaded the final integrated dataset.

- Investigated dataset structure and data quality:
  - Checked missing values and structural missingness.
  - Examined important player, team, and performance variables.
  - Identified variables with incomplete historical information.

- Analyzed season-level characteristics:
  - Checked the number of teams per season.
  - Validated the number of games played per season.
  - Identified non-standard seasons such as lockout seasons and reduced schedules.

- Examined player-team assignment consistency:
  - Investigated possible attribution issues caused by mid-season trades.
  - Compared player minutes assigned to teams with expected team minutes.
  - Checked unusual roster sizes and players appearing for multiple teams within the same season.
## Data Quality and Season Structure Results

The dataset was analyzed for missing values, structural missingness, and season consistency.

### Missing Values

Missing values were mainly found in historical background variables:

- `college`: 2,038 missing values (13.99%)
- `draft_number`: 114 missing values (0.78%)
- `draft_round`: 102 missing values (0.70%)

A within-player analysis showed that these missing values are structurally missing, meaning that they are unavailable for specific players across all recorded seasons rather than missing randomly.

### College Information Analysis

The availability of college information was compared across player groups.

Main findings:

- USA players:
  - 97.1% have college information available
  - 2.9% missing

- Non-USA players:
  - 44.2% have college information available
  - 55.8% missing

The analysis also showed differences depending on career start period, with older player groups having slightly different levels of missing college information.

### Season Structure Analysis

Season length was analyzed to identify irregular NBA seasons.

Most seasons followed the standard 82-game schedule. The following non-standard seasons were detected:

- 1998-99: 50 games (lockout season)
- 2011-12: 66 games (lockout season)
- 2012-13: 81–82 games (minor schedule variation)
- 2019-20: 64–75 games (season interruption)
- 2020-21: 72 games (reduced schedule)

These seasons were kept in the dataset because they represent real NBA season conditions.


## Player-Team Assignment and Trade Consistency Analysis

The notebook investigates possible team attribution issues caused by mid-season trades and players appearing for multiple teams in the same season.

The analysis compares the total player minutes assigned to each team with the expected minutes based on the team's schedule (approximately 240 minutes per game).

Main findings:

- The average roster minutes imbalance was small:
  - Mean imbalance: +0.71%
  - Standard deviation: 4.92%

- Most team-season combinations showed only minor deviations:
  - 51.9% had an imbalance below 2%
  - 26.1% had an imbalance between 2–5%
  - Only 0.3% exceeded 20%

Examples of larger deviations:

- Most over-attributed team:
  - 2022-23 LAC: +17.4% imbalance (19 players)

- Most under-attributed team:
  - 2019-20 GSW: -29.0% imbalance (16 players)

Additional checks:

- Average roster size:
  - Mean: 16.3 players
  - Range: 12–26 players

- 45 team-season combinations contained more than 20 players, mainly due to roster changes.

- 16 player-season cases were identified where players exceeded the team's schedule because they appeared for multiple teams during the same season.

These cases were kept because they represent real NBA roster movements and trades rather than data errors.

# 2. Regression Analysis

Notebook:

`Regression analysis.ipynb`

This notebook investigates the prediction of player performance using regression models.

## Data preparation

- Selected relevant predictor variables from the final dataset.
- Removed variables directly derived from the target variable to avoid information leakage.
- Applied filtering to remove players with very few games.
  - Original observations: 14,569
  - Final observations: 12,073
- Checked correlations and multicollinearity using VIF.
- Removed highly redundant variables before modeling.
- Split the dataset using GroupShuffleSplit based on `player_id` to prevent the same player appearing in both training and test sets.
  - Test size: 30%
  - Random state: 42
## Models used

### Ordinary Least Squares (OLS)

- Fitted a linear regression model.
- Used HC3 robust standard errors.
- Evaluated using R², MAE, and RMSE.

Results:
- Test R²: 0.443
- Test MAE: 6.445
- Test RMSE: 8.106

### Generalized Additive Model (GAM)

- Used GAM to capture non-linear relationships.
- Optimized smoothing parameters using grid search.

Library used:
- `pygam`

Results:
- Test R²: 0.699
- Test MAE: 4.539
- Test RMSE: 5.955

### Random Forest Regression

- Trained a Random Forest regression model.
- Used cross-validation for parameter selection.
- Tuned the non-default parameter:
  - `max_features = 1.0`

All other Random Forest parameters were kept at their default scikit-learn values.

- Evaluated feature importance using permutation importance.

Results:

- Test R²: 0.717
- Test MAE: 4.242
- Test RMSE: 5.776


# 3. Classification Analysis

Notebook:

`Classification.ipynb`

This notebook performs classification tasks using NBA performance variables to predict team playoff qualification.

## Data preparation

- Selected relevant player and team performance variables for classification.
- Removed variables related to the target variable or highly overlapping variables to avoid information leakage and redundancy.

  Examples:
  - Excluded target-related variables (`actual_playoff_team`, team record variables).
  - Removed redundant variables where counting variables already represented the same information (e.g., shooting percentages with shooting attempts/made values).
  - Excluded identifiers and non-predictive variables (`player_id`, `team_id`, `season`, etc.).

- Aggregated player-level statistics into team-season features.
  - Counting variables were converted into per-game averages:
    - Examples: `points`, `assists`, `rebounds`, `three_points_made`, `turnovers`
  - Rate and player characteristic variables were aggregated using minutes-weighted averages:
    - Examples: `usage_pct`, `age`, `player_experience_years`, `height_cm`, `weight_kg`
  - Created the final team-level feature set used for classification.
- Split the dataset into training and test sets using:
  - `train_test_split`
  - Test size: 30%
  - Random state: 42
  - Stratification based on the target variable to preserve class proportions.
## Models used

### Logistic Regression

- Trained baseline logistic regression models.
- Applied regularization:
  - Lasso Logistic Regression (L1)
  - Ridge Logistic Regression (L2)
- Optimized regularization strength (`λ`) using cross-validation.

Results:

| Model | CV AUC | Test AUC | Test Accuracy |
|---|---:|---:|---:|
| Logistic Regression | 0.8892 | 0.9136 | 0.8321 |
| Lasso Logistic (L1) | 0.8903 | 0.9139 | 0.8321 |
| Ridge Logistic (L2) | 0.8890 | 0.9139 | 0.8321 |

Selected regularization parameters:

- Lasso Logistic (L1): λ = 0.0018
- Ridge Logistic (L2): λ = 0.0048

### Random Forest Classification

- Trained a Random Forest classifier.
- Tuned the non-default parameter:
  - `max_features = 0.3`

All other Random Forest parameters were kept at their default values.

- Used cross-validation for parameter selection.
- Evaluated performance using:
  - ROC-AUC
  - Accuracy
  - Confusion Matrix
  - Feature importance analysis

Results:

- CV AUC: 0.849
- Test AUC: 0.8452
- Test Accuracy: 0.7463

Overall, Logistic Regression models achieved the best classification performance, while Random Forest provided additional information about feature importance.

# Tools and Libraries

- Python
- pandas
- numpy
- matplotlib
- scikit-learn
- statsmodels
- pygam
- warning
- pathlib
- scikit-learn
---

# Output

The results generated in this folder include:

- Exploratory data analysis results
- Regression model evaluations
- Classification model evaluations
- Feature importance analyses
- Visualization of model behavior and predictor effects
