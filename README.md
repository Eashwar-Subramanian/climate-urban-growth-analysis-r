# Climate-Driven Urban Growth Analysis (R)

City-level analysis of whether climate signals (rainfall, temperature, humidity) relate to population growth patterns across Australian cities over the 2011–2017 period.

## Research question
Do climate conditions (rainfall, temperature, humidity) show measurable relationships with population change across Australian cities between 2011 and 2017?

## Data used (two-source integration)
**1) Meteorological data**
- Daily weather records for Australian locations
- Variables used: rainfall, temperature, humidity

**2) Population data**
- City-level population values for 2011 and 2017
- Growth computed over the period (city-by-city)

## Method (what I did)
**Data preparation**
- Missing value handling using median imputation (numeric fields)
- Outlier treatment using winsorization
- Standardized types, date formats, and location names for consistent joins

**Integration**
- Temporal alignment of daily weather to a comparable city-level view (aggregation by city and time window)
- Geographic matching of city names across datasets
- Tidy restructuring (e.g., `pivot_longer()`) to keep analysis reproducible and auditable

**Analysis**
- Correlation analysis between climate features and population change
- Time-series exploration of weather patterns per city
- Cross-city comparison of population growth vs aggregated climate signals

## Outputs (what you’ll see in the analysis)
- Correlation matrices: climate variables vs population change
- Time-series plots: climate patterns over the study period
- Scatterplots: population change vs climate signals (city-level)
- City comparison visuals: growth patterns with climate overlays

## Tech stack
- Language: R
- Data wrangling: `dplyr`, `tidyr`
- Visuals: `ggplot2`
- Statistical analysis: correlation + trend exploration

## Feedback I want (please be specific)
If you review this repo, please open a GitHub Issue titled: **Review: climate-driven-urban-growth-analysis** and tell me:
1) Whether the integration logic and assumptions are clear enough to trust the results  
2) Whether the visuals tell a clean story for a recruiter / stakeholder  
3) What you would cut/add to make the findings more decision-useful
