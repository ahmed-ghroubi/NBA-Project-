# Data Preparation and Cleaning

This folder contains all notebooks responsible for collecting, cleaning,
correcting, and combining the raw NBA datasets before the statistical
analysis and machine learning steps.

The complete workflow transforms raw NBA API data into the final
analysis-ready dataset.

## Folder Structure

    code/
    │
    ├── data preparation and cleaning/
       ├── nba_player_performance_raw.ipynb
       ├── load_bio_data.ipynb
       ├── player_career_info.ipynb
       ├── load_clean_team_standings.ipynb
       ├── playoff_team_results.ipynb
       └── merge_final_dataset.ipynb
      

    

# Data Sources

The project uses NBA data collected mainly through the Python package  
`nba_api`.

Main endpoints used:

- `LeagueDashPlayerStats`
    - Regular season player statistics
    - Seasons from 1996-97 onwards

- `LeagueDashPlayerBioStats`
    - Player biography information
    - Height, weight, age, position, draft information, etc.

- `CommonAllPlayers`
    - Historical player information

- `LeagueStandingsV3`
    - Team standings information

Additional playoff information was loaded from historical playoff datasets.

- Sports Reference LLC (n.d.). *NBA & ABA Playoffs Series History*.  
  https://www.basketball-reference.com/playoffs/series.html
------------------------------------------------------------------------

# Data Preparation Workflow

## 1. Loading Raw Player Performance Data

Notebook:

`nba_player_performance_raw.ipynb`

This notebook collects and prepares NBA player performance statistics using the NBA API (`LeagueDashPlayerStats` endpoint).

### Data Collection and Preparation

- Retrieved regular season player statistics for multiple NBA seasons using `nba_api`.
- Combined all seasons into a single dataset.
- Removed unnecessary ranking columns from the raw API output.
- Selected relevant performance variables and renamed columns for a consistent dataset structure.
- Created additional two-point shooting variables from field goal and three-point statistics.

Important variables include:

**Player identification and team information**
- Player and team identifiers (`player_id`, `player_name`, `year`, `season`, `team_id`, `team`, `team_count`)

**Player characteristics**
- Basic player information (`age`)

**Team performance information**
- Season-level team results (`wins`, `losses`, `win_pct`)

**Playing time and basic performance statistics**
- Player activity and traditional statistics (`games_played`, `minutes`, `points`, `field_goals_made`, `field_goal_attempts`, `field_goal_pct`)

**Shooting statistics**
- Shooting performance variables (`two_points_made`, `two_point_attempts`, `two_point_pct`, `three_points_made`, `three_point_attempts`, `three_point_pct`, `free_throws_made`, `free_throw_attempts`, `free_throw_pct`)

**Rebounding and playmaking statistics**
- Defensive and offensive contribution variables (`offensive_rebounds`, `defensive_rebounds`, `rebounds`, `assists`, `turnovers`, `steals`, `blocks`, `blocks_against`)

**Additional performance and impact metrics**
- Advanced player statistics and achievements (`plus_minus`, `nba_fantasy_points`, `double_doubles`, `triple_doubles`)

**Foul statistics**
- Foul-related variables (`personal_fouls`, `personal_fouls_drawn`)

### Output

The cleaned dataset was saved as:

`nba_performance_clean.csv`

and contains processed player-level season statistics prepared for integration with the other NBA datasets.

# 2. Loading Player Biography Data

Notebook:

`load_bio_data.ipynb`

Tasks performed:

- Retrieved player biography information using the NBA API (`LeagueDashPlayerBioStats` endpoint).
- Collected player characteristics across multiple seasons and selected relevant variables for further analysis.
- Cleaned and transformed the dataset by renaming columns, converting units (height: inches → centimeters, weight: pounds → kilograms), and standardizing the structure for later merging.

### Important variables include:

**Player identification and team information**
- Player identifiers and season information (`player_id`, `player_name`, `year`, `season`, `team_id`, `team`)

**Player characteristics**
- Physical and background information (`age`, `height_cm`, `weight_kg`, `college`, `country`)

**Draft information**
- Draft-related variables (`draft_year`, `draft_round`, `draft_number`)

**Basic performance statistics**
- Traditional player statistics (`games_played`, `points`, `rebounds`, `assists`)

**Advanced performance metrics**
- Efficiency and role-related statistics (`net_rating`, `usage_pct`, `true_shooting_pct`, `assist_pct`, `offensive_rebound_pct`, `defensive_rebound_pct`)


### Manual Corrections and Data Validation 

Some NBA API values contained missing or inconsistent information.

The correction process included:

### Draft information

The NBA API contained inconsistent draft information for three players. These records were checked manually and corrected using external player/draft references.

| Player | Player ID | Corrected draft year | Draft round | Draft number | Reference |
|---|---:|---:|---|---|---|
| Jerry Smith | 202536 | 2010 | Undrafted | Undrafted | [RealGM](http://basketball.realgm.com/player/Jerry-Smith/Summary/2391) |
| Walker Russell Jr. | 201041 | 2006 | Undrafted | Undrafted | [RealGM](http://basketball.realgm.com/player/Walker-Russell/Summary/1967) |
| Mark Jones | 2891 | 1998 | Undrafted | Undrafted | [The Draft Review](http://www.thedraftreview.com/index.php?option=com_content&view=article&id=14076) |

### Missing height and weight

Missing height and weight values are first filled from another season of the **same player** using forward/backward filling. Only players for whom no value exists in any observed season are corrected manually.

The remaining missing values were manually verified using the corresponding
Basketball-Reference player pages.

| Player | Player ID | Height used | Weight used | Basketball-Reference |
|---|---:|---:|---:|---|
| Ratko Varda | 2237 | 216 cm | 117 kg | [Profile](https://www.basketball-reference.com/players/v/vardara01.html) |
| Jerrelle Benimon | 203968 | 203 cm | 111 kg | [Profile](https://www.basketball-reference.com/players/b/benimje01.html) |
| Khalid El-Amin | 2064 | 178 cm | 90 kg | [Profile](https://www.basketball-reference.com/players/e/elamikh01.html) |
| Ruben Garces | 2092 | 206 cm | 111 kg | [Profile](https://www.basketball-reference.com/players/g/garceru01.html) |
| Terrance Roberson | 2121 | 201 cm | 97 kg | [Profile](https://www.basketball-reference.com/players/r/roberte01.html) |
| Will Cherry | 203805 | 183 cm | 83 kg | [Profile](https://www.basketball-reference.com/players/c/cherrwi01.html) |
| Chris Youngblood | 1642959 | 193 cm | 100 kg | [Profile](https://www.basketball-reference.com/players/y/youngch01.html) |
| Jahmyl Telfort | 1643141 | 201 cm | 102 kg | [Profile](https://www.basketball-reference.com/players/t/telfoja01.html) |
| Jaylen Wells | 1642377 | 201 cm | 93 kg | [Profile](https://www.basketball-reference.com/players/w/wellsja01.html) |
| LJ Cryer | 1643018 | 185 cm | 91 kg | [Profile](https://www.basketball-reference.com/players/c/cryerlj01.html) |
| Lawson Lovering | 1643133 | 213 cm | 111 kg | [Profile](https://www.basketball-reference.com/players/l/loverla01.html) |
| Tolu Smith | 1642449 | 211 cm | 115 kg | [Profile](https://www.basketball-reference.com/players/s/smithto05.html) |
| Patrick Christopher | 203565 | 196 cm | 94 kg | [Profile](https://www.basketball-reference.com/players/c/chrispa01.html) |
| Jamal Robinson | 1554 | 201 cm | 96 kg | [Profile](https://www.basketball-reference.com/players/r/robinja03.html) |
| Ike Fontaine | 1829 | 193 cm | 95 kg | [Profile](https://www.basketball-reference.com/players/f/fontais01.html) |
| Reggie Williams | 199 | 201 cm | 86 kg | [Profile](https://www.basketball-reference.com/players/w/willire01.html) |

The notebook records and displays the original values of manually corrected players (height, weight, draft year, draft round, and draft number) before applying the corrections, allowing comparison with the updated dataset.
### Output

The cleaned dataset was saved as:

`bio_info_clean.csv`

and contains standardized player biography information, including player characteristics, draft information, and selected performance variables.

# 3. Player Career Information

Notebook:

`player_career_info.ipynb`

This notebook collects and prepares historical player information using the NBA API (`CommonAllPlayers` endpoint).

### Data Collection and Preparation

- Retrieved historical player information and career-related attributes.
- Filtered and prepared player records to match the project time period.
- Standardized player identifiers (`player_id`) to enable consistent merging with the other datasets.
- Prepared career timeline information required for dataset integration.

Important variables include:

**Player identification**
- Player identifiers and names (`player_id`, `player_name`)

**Career timeline information**
- Career start and end years (`career_start_year`, `career_end_year`)

**Roster information**
- Player activity status (`roster_status`)

---

**Manual Corrections**

Some career information obtained from the NBA data contained inconsistencies.

**Mark Jones career start year**

The NBA data contained an incorrect career start year for Mark Jones (`player_id = 2891`). The notebook displays the original value before it was manually corrected to **2005** based on his documented career history as well.

Reference:
Sports Reference LLC (n.d.). *Mark Jones Stats, Height, Weight, Position, Draft Status and more*.  
https://www.basketball-reference.com/players/j/jonesma03.html

### Output

The cleaned dataset was saved as:

`player_career_info_clean.csv`

and contains standardized historical player information and career-related variables prepared for merging with the other datasets.
## 4. Team Standings Data

Notebook:

`load_clean_team_standings.ipynb`

This notebook collects and prepares NBA team standings information using the NBA API (`LeagueStandingsV3` endpoint).

### Data Collection and Preparation

- Retrieved regular season team standings data for NBA seasons from **1996-97 to 2025-26** using `nba_api`.
- Combined all seasons into a single dataset and stored the raw API output.
- Selected relevant team performance variables and renamed columns for a consistent dataset structure.
- Created a full team name variable by combining team city and team name.

Important variables include:

**Team identification and season information**
- Team and season identifiers (`year`, `season`, `team_id`, `team_full_name`)

**Team classification information**
- Team grouping variables (`conference`, `division`)

**Team performance statistics**
- Season-level team results (`team_wins`, `team_losses`, `team_win_pct`)

**Offensive and defensive performance metrics**
- Team scoring and efficiency-related variables (`team_points_per_game`, `team_opp_points_per_game`, `team_point_diff_per_game`)

### Output

The cleaned dataset was saved as:

`team_standings_clean.csv`

and contains season-level team performance information prepared for integration with the player datasets.

# 5. Playoff Results

Notebook:

`playoff_team_results.ipynb`

This notebook loads and prepares historical NBA playoff series information to create team-level playoff outcome variables from Sports Reference LLC (n.d.), *NBA & ABA Playoffs Series History*:  
https://www.basketball-reference.com/playoffs/series.html

### Data Collection and Preparation

- Loaded historical playoff series data from Sports Reference (NBA & ABA Playoffs Series History).
- Selected relevant playoff series information, including winner/loser teams and series results.
- Filtered the dataset to include only NBA playoff series from **1997 to 2026**.
- Extracted playoff seeds and removed seed information from team names.
- Added conference information based on playoff series.
- Converted playoff series results into a team-level dataset containing playoff outcomes.

Important variables include:

**Team and season information**
- Team and season identifiers (`year`, `season`, `team_full_name`, `conference`)

**Playoff participation information**
- Playoff qualification indicators (`actual_playoff_team`, `top_8_conference_team`)

**Playoff outcome information**
- Team postseason results (`playoff_stage`, `playoff_finish_rank`, `playoff_seed`)

**Achievement indicators**
- Playoff success variables (`reached_conference_semifinals`, `reached_conference_finals`, `reached_finals`, `won_nba_title`)

### Output

The cleaned dataset was saved as:

`playoff_team_results_clean.csv`

and contains team-level playoff results and postseason performance indicators prepared for integration with the other NBA datasets.

------------------------------------------------------------------------

# 6. Final Dataset Creation

Notebook:

`merge_final_dataset.ipynb`

This notebook combines all cleaned datasets into one final analysis-ready dataset.

### Data Integration Steps

- Loaded cleaned player performance data, biography information, career information, team standings, and playoff results.
- Merged datasets using common identifiers (`player_id`, `team_id`, and season information).
- Aggregated team-level information before merging it with player-level data.

### Data Selection

Some playoff outcome variables were excluded from the final dataset because they were not required for the current analysis.

Excluded variables include:

- `top_8_conference_team`
- `playoff_seed`
- `playoff_stage`
- `playoff_finish_rank`
- `reached_conference_semifinals`
- `reached_conference_finals`
- `reached_finals`
- `won_nba_title`

These variables were kept separately and can be used for future research questions involving playoff prediction or postseason success.

### Data Cleaning

During the merging process, special handling was required for team naming inconsistencies, especially the **Los Angeles teams**, where different abbreviations/names were used across datasets. These inconsistencies were corrected to ensure proper matching between player, team, and playoff datasets.

### Output

The final dataset was saved as:

`nba_final_player_season_data.csv`

and combines player-level, team-level, and historical information into a single dataset ready for further analysis.

# Final Data Processing Pipeline

    NBA API / Raw Sources
              |
              v
    Raw Player Statistics
    Raw Biography Data
    Team Data
    Playoff Data
              |
              v
    Cleaning & Standardization
              |
              v
    Manual Corrections
              |
              v
    Dataset Merging
              |
              v
    Final Analysis Dataset
              |
              v
    Used as input for further analysis

------------------------------------------------------------------------

# Tools and Libraries

-   Python
-   pandas
-   numpy
-   nba_api
-   pathlib
-   difflib
-   time
