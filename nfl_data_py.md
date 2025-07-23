# nfl_data_py Function and Dataset Catalog

The `nfl_data_py` Python package is a powerful tool for accessing and analyzing NFL data, drawing from various sources including nflfastR, PFR, and the NFL's Next Gen Stats. This catalog provides a detailed overview of its datasets and functions.

---

## Play-by-Play Data

### `import_pbp_data`

Fetches detailed play-by-play data for NFL games.

-   **Function Signature:** `import_pbp_data(years, columns=None, downcast=True, cache=False, alt_path=None)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides granular data for every play in regular and postseason games. This is the foundational dataset for many advanced analytics, including Expected Points Added (EPA) and Win Probability Added (WPA).
-   **Parameters / Filters:**
    -   `years` (List[int]): Required. Seasons to fetch (e.g., `[2023]`, `range(2020, 2024)`).
    -   `columns` (List[str]): Optional. Specific columns to return; fetches all if None.
    -   `downcast` (bool): Optional. If True, downcasts data types (e.g., float64 to float32) to save memory.
    -   `cache` (bool): Optional. If True, uses cached data if available.
    -   `alt_path` (str): Optional. Specifies an alternative path for cached data.
-   **Fields / Columns:** Includes hundreds of columns detailing the game state, participants, play type, outcomes, EPA, WPA, and more. Use `nfl_data_py.see_pbp_cols()` for a full list.
-   **Historical Coverage:** 1999 to present.
-   **Update Frequency:** Weekly during the season, typically Tuesday mornings.
-   **Data Source / Provenance:** nflfastR (sourced originally from the NFL's proprietary data feeds).
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    pbp_2023 = nfl.import_pbp_data([2023])
    ```
-   **Caveats:** Datasets are large. Downcasting is recommended for memory efficiency. Data from 1999 to 2010 is less detailed than data from 2011 onwards.

---

## Player Seasonal and Weekly Statistics

### `import_seasonal_data`

Fetches season-level aggregate statistics for players.

-   **Function Signature:** `import_seasonal_data(years, s_type='REG')`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides summarized seasonal statistics for offensive players (passing, rushing, receiving), including fantasy points (PPR, Standard, Half-PPR) and various usage metrics.
-   **Parameters / Filters:**
    -   `years` (List[int]): Required. Seasons to fetch.
    -   `s_type` (str): Optional. Season type: 'REG' (default), 'POST', or 'ALL'.
-   **Fields / Columns:** `player_id`, `season`, `season_type`, `completions`, `attempts`, `passing_yards`, `rushing_yards`, `targets`, `receptions`, various `fantasy_points` metrics.
-   **Historical Coverage:** 1999 to present.
-   **Update Frequency:** Weekly during the season.
-   **Data Source / Provenance:** Calculated from nflfastR play-by-play data.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    seasonal_stats = nfl.import_seasonal_data([2023], s_type='REG')
    ```

***

### `import_weekly_data`

Fetches week-level statistics for players.

-   **Function Signature:** `import_weekly_data(years, columns=None, downcast=True)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides game-level statistics for offensive players, mirroring the seasonal data but broken down by week.
-   **Parameters / Filters:**
    -   `years` (List[int]): Required. Seasons to fetch.
    -   `columns` (List[str]): Optional. Specific columns to return.
    -   `downcast` (bool): Optional. If True, downcasts data types to save memory.
-   **Fields / Columns:** `player_id`, `season`, `week`, `opponent_team`, `completions`, `passing_yards`, `rushing_yards`, `receptions`, `fantasy_points_ppr`, etc. Use `nfl_data_py.see_weekly_cols()` for a full list.
-   **Historical Coverage:** 1999 to present.
-   **Update Frequency:** Weekly during the season.
-   **Data Source / Provenance:** Calculated from nflfastR play-by-play data.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    weekly_2023 = nfl.import_weekly_data([2023])
    ```

---

## Roster and Personnel Data

### `import_rosters`

Fetches seasonal roster data.

-   **Function Signature:** `import_rosters(years, columns=None)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides a list of players who were on an NFL roster during the specified seasons, including biographical and status information.
-   **Parameters / Filters:**
    -   `years` (List[int]): Required. Seasons to fetch.
    -   `columns` (List[str]): Optional. Specific columns to return.
-   **Fields / Columns:** `season`, `team`, `position`, `player_name`, `player_id`, `gsis_id`, `birth_date`, `height`, `weight`, `college`, `status`.
-   **Historical Coverage:** 1999 to present.
-   **Update Frequency:** Updated regularly, especially during the active season.
-   **Data Source / Provenance:** nflverse (MFL and nflfastR roster data).
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    rosters_2024 = nfl.import_rosters([2024])
    ```

***

### `import_weekly_rosters`

Fetches rosters for specific games/weeks.

-   **Function Signature:** `import_weekly_rosters(years, columns=None)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides information on which players were active for specific teams on a weekly basis.
-   **Parameters / Filters:**
    -   `years` (List[int]): Required. Seasons to fetch.
    -   `columns` (List[str]): Optional. Specific columns to return.
-   **Fields / Columns:** Includes player identifiers (`gsis_id`, `pfr_id`, etc.), `season`, `week`, `game_id`, `team`, `player_name`, and position.
-   **Historical Coverage:** 1999 to present.
-   **Update Frequency:** Weekly during the season.
-   **Data Source / Provenance:** nflverse.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    weekly_rosters_2023 = nfl.import_weekly_rosters([2023])
    ```

***

### `import_depth_charts`

Fetches team depth charts.

-   **Function Signature:** `import_depth_charts(years)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides weekly depth chart information for NFL teams.
-   **Parameters / Filters:**
    -   `years` (List[int]): Optional. Seasons to fetch.
-   **Fields / Columns:** `season`, `week`, `team`, `formation`, `depth_team`, `position`, `player_name`, various player IDs.
-   **Historical Coverage:** 2001 to present.
-   **Update Frequency:** Weekly during the season.
-   **Data Source / Provenance:** nflverse (sourced from various outlets).
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    depth_charts_2024 = nfl.import_depth_charts([2024])
    ```

***

### `import_injuries`

Fetches historical injury reports.

-   **Function Signature:** `import_injuries(years)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Data regarding player injuries as reported by teams weekly.
-   **Parameters / Filters:**
    -   `years` (List[int]): Optional. Seasons to fetch.
-   **Fields / Columns:** `season`, `week`, `team`, `player_name`, `position`, `injury`, `practice_status`, `game_status` (e.g., Questionable, Out).
-   **Historical Coverage:** 2009 to present.
-   **Update Frequency:** Weekly during the season.
-   **Data Source / Provenance:** nflverse.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    injuries_2023 = nfl.import_injuries([2023])
    ```

---

## Next Gen Stats (NGS)

### `import_ngs_data`

Fetches NFL Next Gen Stats data.

-   **Function Signature:** `import_ngs_data(stat_type, years=None)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Accesses advanced metrics derived from player tracking data, such as air yards, time to throw, and rushing yards over expectation.
-   **Parameters / Filters:**
    -   `stat_type` (str): Required. Type of stats: 'passing', 'rushing', or 'receiving'.
    -   `years` (List[int]): Optional. Seasons to fetch.
-   **Fields / Columns:** Varies by `stat_type`.
    -   *Passing:* `avg_time_to_throw`, `avg_intended_air_yards`, `aggressiveness`, `completion_percentage_above_expectation`.
    -   *Rushing:* `efficiency` (north/south running), `percent_attempts_gte_8_defenders`, `rushing_yards_over_expected`.
    -   *Receiving:* `avg_cushion`, `avg_separation`, `avg_intended_air_yards`, `catch_percentage`.
-   **Historical Coverage:** 2016 to present.
-   **Update Frequency:** Weekly during the season.
-   **Data Source / Provenance:** NFL Next Gen Stats via nflverse.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    ngs_passing = nfl.import_ngs_data('passing', years=[2023])
    ```

---

## Pro-Football-Reference (PFR) Data

### `import_seasonal_pfr`

Fetches advanced seasonal data scraped from Pro-Football-Reference.

-   **Function Signature:** `import_seasonal_pfr(s_type, years=None)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides advanced season-aggregated metrics that are not available in standard PBP feeds, such as pressures, blitzes faced, and detailed receiving stats.
-   **Parameters / Filters:**
    -   `s_type` (str): Required. Stat category: 'pass', 'rec', or 'rush'.
    -   `years` (List[int]): Optional. Seasons to fetch.
-   **Fields / Columns:** Varies by `s_type`. Includes `player_id`, `season`, and advanced metrics like `blitzes`, `pressures`, `hurries`, `drops`, `batted_passes`.
-   **Historical Coverage:** 2018 to present for advanced stats.
-   **Update Frequency:** Annually/periodically.
-   **Data Source / Provenance:** Pro-Football-Reference.com (PFR).
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    pfr_passing = nfl.import_seasonal_pfr('pass', [2023])
    ```
-   **Caveats:** Data is scraped from PFR; respect their rate limits if accessing data frequently. Structure may change if PFR updates its website.

***

### `import_weekly_pfr`

Fetches advanced weekly (gamelog) data scraped from Pro-Football-Reference.

-   **Function Signature:** `import_weekly_pfr(s_type, years=None)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides advanced game-level metrics sourced from PFR's advanced gamelogs.
-   **Parameters / Filters:**
    -   `s_type` (str): Required. Stat category: 'pass', 'rec', or 'rush'.
    -   `years` (List[int]): Optional. Seasons to fetch.
-   **Fields / Columns:** Similar to seasonal PFR data, but includes `week`, `game_id`, and game-specific advanced metrics.
-   **Historical Coverage:** 2018 to present.
-   **Update Frequency:** Weekly during the season.
-   **Data Source / Provenance:** Pro-Football-Reference.com (PFR).
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    pfr_receiving_wk = nfl.import_weekly_pfr('rec', [2023])
    ```
-   **Caveats:** Subject to PFR rate limits and website structure.

***

### `import_snap_counts`

Fetches player snap count data.

-   **Function Signature:** `import_snap_counts(years)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides the number and percentage of snaps (offensive, defensive, special teams) played by players in specific games.
-   **Parameters / Filters:**
    -   `years` (List[int]): Optional. Seasons to fetch.
-   **Fields / Columns:** `game_id`, `pfr_game_id`, `player`, `pfr_player_id`, `team`, `offense_snaps`, `offense_pct`, `defense_snaps`, `st_snaps`.
-   **Historical Coverage:** 2012 to present.
-   **Update Frequency:** Weekly during the season.
-   **Data Source / Provenance:** PFR via nflverse.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    snaps_2023 = nfl.import_snap_counts([2023])
    ```

---

## Draft and Combine Data

### `import_draft_picks`

Fetches historical NFL draft pick information.

-   **Function Signature:** `import_draft_picks(years=None)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Details of every player drafted, including their pick number, team, and college.
-   **Parameters / Filters:**
    -   `years` (List[int]): Optional. Draft years to fetch.
-   **Fields / Columns:** `season`, `round`, `pick`, `team`, `player_name`, `position`, `college`, various player IDs (PFR, sleeper, MFL).
-   **Historical Coverage:** 1936 to the most recent draft.
-   **Update Frequency:** Annually after the NFL draft.
-   **Data Source / Provenance:** nflverse/DynastyProcess.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    draft_2024 = nfl.import_draft_picks([2024])
    ```

***

### `import_draft_values`

Fetches valuations for draft picks based on different models.

-   **Function Signature:** `import_draft_values()`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides trade value estimates for draft picks (e.g., Jimmy Johnson model, Fitzgerald-Spielberger).
-   **Parameters / Filters:** None.
-   **Fields / Columns:** `pick`, `draft_year`, and columns for various valuation models (e.g., `stuart`, `jj`, `hill`).
-   **Historical Coverage:** Static valuation tables.
-   **Update Frequency:** Periodically updated as new models gain prominence.
-   **Data Source / Provenance:** Various sources via nflverse/DynastyProcess.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    pick_values = nfl.import_draft_values()
    ```

***

### `import_combine_data`

Fetches NFL Scouting Combine results.

-   **Function Signature:** `import_combine_data(years=None, positions=None)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Data from the NFL combine, including physical measurements and drill results.
-   **Parameters / Filters:**
    -   `years` (List[int]): Optional. Combine years to fetch.
    -   `positions` (List[str]): Optional. Filter by positions (e.g., `['QB', 'WR']`).
-   **Fields / Columns:** `season`, `player_name`, `pos`, `ht`, `wt`, `forty`, `bench`, `vertical`, `broad_jump`, `cone`, `shuttle`, various player IDs.
-   **Historical Coverage:** 2000 to present.
-   **Update Frequency:** Annually after the NFL Combine.
-   **Data Source / Provenance:** Pro-Football-Reference and Draft Scout via nflverse.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    combine_2024_qbs = nfl.import_combine_data([2024], positions=['QB'])
    ```

---

## Team, Schedule, and Game Information

### `import_team_desc`

Fetches team descriptions, logos, and colors.

-   **Function Signature:** `import_team_desc()`
-   **Return Type:** Pandas DataFrame
-   **Description:** A dataset containing metadata for NFL teams.
-   **Parameters / Filters:** None.
-   **Fields / Columns:** `team_abbr`, `team_name`, `team_id`, `team_color`, `team_color2`, `team_logo_wikipedia`, `team_logo_espn`, `team_wordmark`.
-   **Historical Coverage:** Current team information, including historical teams/names (e.g., OAK/LV, SD/LAC).
-   **Update Frequency:** Periodically, when teams rebrand or relocate.
-   **Data Source / Provenance:** nflfastR team data.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    teams = nfl.import_team_desc()
    ```

***

### `import_schedules`

Fetches NFL game schedules.

-   **Function Signature:** `import_schedules(years)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides details about past and future NFL games, including results and betting odds.
-   **Parameters / Filters:**
    -   `years` (List[int]): Required. Seasons to fetch.
-   **Fields / Columns:** `game_id`, `season`, `game_type`, `week`, `gameday`, `weekday`, `gametime`, `away_team`, `home_team`, `away_score`, `home_score`, `spread_line`, `total_line`, `overtime`, `div_game`.
-   **Historical Coverage:** 1999 to the current season.
-   **Update Frequency:** Updated regularly for scores and upcoming game info.
-   **Data Source / Provenance:** nflfastR/nflverse.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    schedule_2024 = nfl.import_schedules([2024])
    ```

***

### `import_officials`

Fetches information about officials for NFL games.

-   **Function Signature:** `import_officials(years=None)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Lists the officiating crews for NFL games.
-   **Parameters / Filters:**
    -   `years` (List[int]): Optional. Seasons to fetch.
-   **Fields / Columns:** `game_id`, `season`, `week`, `official_name`, `position` (e.g., Referee, Umpire).
-   **Historical Coverage:** 2011 to present.
-   **Update Frequency:** Weekly during the season.
-   **Data Source / Provenance:** nflverse.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    officials_2023 = nfl.import_officials([2023])
    ```

---

## Betting and Lines Data

### `import_win_totals`

Fetches preseason win total lines.

-   **Function Signature:** `import_win_totals(years=None)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides the Vegas/sportsbook over/under lines for team win totals before the season starts.
-   **Parameters / Filters:**
    -   `years` (List[int]): Optional. Seasons to fetch.
-   **Fields / Columns:** `season`, `team`, `win_total`, `vig_over`, `vig_under`.
-   **Historical Coverage:** 2002 up to 2020 (see caveats).
-   **Update Frequency:** Historically annual.
-   **Data Source / Provenance:** Various betting sources via nflverse.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    win_totals_2020 = nfl.import_win_totals([2020])
    ```
-   **Caveats:** As of recent checks, the underlying data source for this function appears inactive, and data may not be available past 2020.

---

## Miscellaneous Data

### `import_qbr`

Fetches ESPN's QBR (Total Quarterback Rating).

-   **Function Signature:** `import_qbr(years=None, level='nfl', frequency='season')`
-   **Return Type:** Pandas DataFrame
-   **Description:** Retrieves ESPN’s proprietary QBR metric for NFL and college quarterbacks.
-   **Parameters / Filters:**
    -   `years` (List[int]): Optional. Seasons to fetch.
    -   `level` (str): Optional. 'nfl' (default) or 'college'.
    -   `frequency` (str): Optional. 'season' (default) or 'weekly'.
-   **Fields / Columns:** `season`, `team_abb`, `player_name`, `qbr_total`, `pts_added`, `qb_plays`, metrics broken down by pass, run, penalty, etc.
-   **Historical Coverage:** NFL since 2006; College since 2004.
-   **Update Frequency:** Weekly during the season.
-   **Data Source / Provenance:** ESPN Stats & Information Group via nflverse.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    qbr_2023 = nfl.import_qbr(years=[2023], frequency='season')
    ```

***

### `import_ftn_data`

Fetches charting data from FTN (Fantasy Points Data).

-   **Function Signature:** `import_ftn_data(years, columns=None, downcast=True, thread_requests=False)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Provides access to a subset of FTN's manually charted play data, offering deeper insights than standard PBP.
-   **Parameters / Filters:**
    -   `years` (List[int]): Required. Seasons to fetch.
    -   `columns` (List[str]): Optional.
    -   `downcast` (bool): Optional.
    -   `thread_requests` (bool): Optional.
-   **Fields / Columns:** Includes standard `nflverse_game_id`, `play_id`, plus detailed charting like `is_pocket_location_fumbled`, `qb_was_pressured`, `route`, `targeted_depth_of_target`.
-   **Historical Coverage:** 2022 to present.
-   **Update Frequency:** Within 48 hours following each game.
-   **Data Source / Provenance:** FTN Data via nflverse.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    ftn_charting_2023 = nfl.import_ftn_data([2023])
    ```

***

### `import_ids`

Fetches a mapping of player IDs across different platforms.

-   **Function Signature:** `import_ids(columns=None, ids=None)`
-   **Return Type:** Pandas DataFrame
-   **Description:** A crucial resource for linking datasets. It provides mappings between various ID systems (GSIS, PFR, ESPN, MFL, Sleeper, etc.).
-   **Parameters / Filters:**
    -   `columns` (List[str]): Optional. Specific ID columns to return.
    -   `ids` (List[str]): Optional. Specific IDs to look up.
-   **Fields / Columns:** `name`, `team`, `position`, and various ID columns: `mfl_id`, `sportradar_id`, `gsis_id`, `pfr_id`, `espn_id`, `sleeper_id`, etc.
-   **Historical Coverage:** Comprehensive historical and current players.
-   **Update Frequency:** Regularly updated.
-   **Data Source / Provenance:** nflverse/DynastyProcess player ID map.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    id_map = nfl.import_ids()
    ```

---

## Utility Functions

### `see_pbp_cols`

Lists available columns in the play-by-play dataset.

-   **Function Signature:** `see_pbp_cols()`
-   **Return Type:** List of strings (or similar iterable)
-   **Description:** Helper function to view all column names available via `import_pbp_data()`.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    pbp_columns = nfl.see_pbp_cols()
    ```

***

### `see_weekly_cols`

Lists available columns in the weekly dataset.

-   **Function Signature:** `see_weekly_cols()`
-   **Return Type:** List of strings (or similar iterable)
-   **Description:** Helper function to view all column names available via `import_weekly_data()`.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    weekly_columns = nfl.see_weekly_cols()
    ```

***

### `clean_nfl_data`

Utility for standardizing player names and fixing common data entry issues.

-   **Function Signature:** `clean_nfl_data(df)`
-   **Return Type:** Pandas DataFrame
-   **Description:** Takes a raw DataFrame (usually PBP or roster data) and cleans common inconsistencies in player names (e.g., "J. Smith" vs "John Smith") and other known data quirks.
-   **Parameters / Filters:**
    -   `df` (Pandas DataFrame): The dataframe to clean.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    roster = nfl.import_rosters([2023])
    cleaned_roster = nfl.clean_nfl_data(roster)
    ```

***

### `cache_pbp`

Locally caches play-by-play data for faster retrieval.

-   **Function Signature:** `cache_pbp(years, downcast=True, alt_path=None)`
-   **Return Type:** None
-   **Description:** Downloads and stores PBP data locally. Subsequent calls to `import_pbp_data` with `cache=True` will load from this local store, speeding up access.
-   **Parameters / Filters:**
    -   `years` (List[int]): Required. Seasons to cache.
    -   `downcast` (bool): Optional.
    -   `alt_path` (str): Optional.
-   **Caveats:** If used during the season, the cache needs to be refreshed weekly to include the latest games.
-   **Usage Example:**
    ```python
    import nfl_data_py as nfl
    nfl.cache_pbp([2023])
    ```
