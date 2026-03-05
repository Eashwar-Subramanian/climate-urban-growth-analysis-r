# Climate-Driven Urban Growth Analysis (R)

I’m testing whether **city-level climate patterns** (rainfall, temperature, humidity) are associated with **urban population growth** across Australian cities between **2011 → 2017**.

This repo intentionally keeps the artefacts simple: the **two source datasets** + a **fully reproducible R workflow (included below in this README)**.

---

## Research question

**Does climate (rainfall, temperature, humidity) influence population growth patterns across Australian cities between 2011 and 2017?**

---

## Data (in this repo)

- `City Dataset.csv`  
  City population (2011 Census) and June 2017 population + growth %.
- `Weather Dataset.csv`  
  Daily weather observations for Australian locations (used to aggregate climate features over 2011–2017).

---

## What I do in the analysis

1. Clean + parse the city dataset (numbers, % growth)
2. Clean + parse the weather dataset
3. Aggregate daily climate into city-level summaries (2011–2017)
4. Join climate summaries to city growth
5. Evaluate relationships (correlations + simple regression)
6. Produce plots for quick interpretation

---

## Reproduce (run exactly as-is)

### Requirements
- R (tested with tidyverse-style workflow)
- Packages: `readr`, `dplyr`, `tidyr`, `stringr`, `lubridate`, `ggplot2`, `broom`

### Run

Open R/RStudio in the repo root and run the code below:

```r
# -------------------------------
# Climate vs Urban Growth (R)
# -------------------------------

packages <- c("readr","dplyr","tidyr","stringr","lubridate","ggplot2","broom")
installed <- rownames(installed.packages())
for (p in packages) if (!p %in% installed) install.packages(p, dependencies = TRUE)

library(readr)
library(dplyr)
library(tidyr)
library(stringr)
library(lubridate)
library(ggplot2)
library(broom)

# ---------- Helpers ----------
to_num <- function(x) readr::parse_number(as.character(x))
to_pct <- function(x) readr::parse_number(str_replace_all(as.character(x), "%", "")) / 100

pick_col <- function(df, patterns, required = TRUE) {
  nms <- names(df)
  hits <- which(str_detect(tolower(nms), str_c(patterns, collapse = "|")))
  if (length(hits) == 0) {
    if (required) stop(paste0(
      "Could not find a required column using patterns: ",
      paste(patterns, collapse = ", "),
      "\nAvailable columns:\n- ",
      paste(nms, collapse = "\n- ")
    ))
    return(NA_character_)
  }
  nms[hits[1]]
}

dir.create("outputs", showWarnings = FALSE)

# ---------- Load City dataset ----------
city_raw <- read_csv("City Dataset.csv", show_col_types = FALSE)

# Clean column names (remove bracket footnotes like [2], [3])
names(city_raw) <- names(city_raw) |>
  str_replace_all("\\[.*?\\]", "") |>
  str_replace_all("\\s+", " ") |>
  str_trim()

# Identify key columns in city dataset
col_city   <- pick_col(city_raw, c("gccsa/sua","city"))
col_2011   <- pick_col(city_raw, c("2011","census"))
col_2017   <- pick_col(city_raw, c("june 2017"))
col_growth <- pick_col(city_raw, c("^growth$","growth"))

city <- city_raw |>
  transmute(
    city = as.character(.data[[col_city]]),
    population_2011 = to_num(.data[[col_2011]]),
    population_2017 = to_num(.data[[col_2017]]),
    growth_pct = to_pct(.data[[col_growth]])
  ) |>
  filter(!is.na(city), !is.na(population_2011), !is.na(population_2017))

# ---------- Load Weather dataset ----------
weather_raw <- read_csv("Weather Dataset.csv", show_col_types = FALSE)

# Try to detect columns in weather dataset robustly
# (works across common Kaggle weather schema variants)
col_loc  <- pick_col(weather_raw, c("location","city","town","station","place","name"))
col_date <- pick_col(weather_raw, c("^date$","date","day","datetime"))
# rainfall / temperature / humidity can vary (Rainfall, Rain, Precip; Temp, MinTemp, MaxTemp; Humidity, Humid)
rain_cols <- names(weather_raw)[str_detect(tolower(names(weather_raw)), "rain|precip")]
temp_cols <- names(weather_raw)[str_detect(tolower(names(weather_raw)), "temp|temperature")]
hum_cols  <- names(weather_raw)[str_detect(tolower(names(weather_raw)), "humid")]

if (length(rain_cols) == 0) stop("No rainfall-like columns found in Weather Dataset.csv (expected name containing 'rain' or 'precip').")
if (length(temp_cols) == 0) stop("No temperature-like columns found in Weather Dataset.csv (expected name containing 'temp' or 'temperature').")
if (length(hum_cols) == 0)  message("No humidity-like columns found. The analysis will run without humidity features.")

weather <- weather_raw |>
  mutate(
    location = as.character(.data[[col_loc]]),
    date = suppressWarnings(as.Date(.data[[col_date]]))
  ) |>
  filter(!is.na(location), !is.na(date)) |>
  filter(date >= as.Date("2011-01-01"), date <= as.Date("2017-12-31"))

# Standardize numeric parsing for selected climate columns
for (cname in unique(c(rain_cols, temp_cols, hum_cols))) {
  weather[[cname]] <- suppressWarnings(to_num(weather[[cname]]))
}

# Aggregate to city-level climate summaries (2011–2017)
climate_city <- weather |>
  group_by(location) |>
  summarise(
    days = n(),
    rainfall_mean = mean(across(all_of(rain_cols)), na.rm = TRUE) |> mean(na.rm = TRUE),
    temperature_mean = mean(across(all_of(temp_cols)), na.rm = TRUE) |> mean(na.rm = TRUE),
    humidity_mean = if (length(hum_cols) > 0) mean(across(all_of(hum_cols)), na.rm = TRUE) |> mean(na.rm = TRUE) else NA_real_,
    .groups = "drop"
  ) |>
  rename(city = location)

# Join + measure match rate
joined <- city |>
  left_join(climate_city, by = "city")

match_rate <- mean(!is.na(joined$rainfall_mean) & !is.na(joined$temperature_mean))
writeLines(
  paste0("City→weather match rate (has rainfall + temperature): ", round(match_rate * 100, 2), "%"),
  con = "outputs/match_rate.txt"
)

# ---------- Correlations ----------
corr_vars <- joined |>
  select(growth_pct, rainfall_mean, temperature_mean, humidity_mean) |>
  drop_na(growth_pct)

corr_mat <- cor(corr_vars, use = "pairwise.complete.obs")
write.csv(corr_mat, "outputs/correlation_matrix.csv", row.names = TRUE)

# ---------- Simple regression ----------
# Growth as a function of climate means (runs even if humidity is missing)
reg_df <- joined |>
  select(growth_pct, rainfall_mean, temperature_mean, humidity_mean) |>
  drop_na(growth_pct, rainfall_mean, temperature_mean)

fit <- if (all(is.na(reg_df$humidity_mean))) {
  lm(growth_pct ~ rainfall_mean + temperature_mean, data = reg_df)
} else {
  lm(growth_pct ~ rainfall_mean + temperature_mean + humidity_mean, data = reg_df)
}

reg_summary <- broom::tidy(fit)
write.csv(reg_summary, "outputs/regression_summary.csv", row.names = FALSE)

# ---------- Plots ----------
p1 <- ggplot(joined, aes(x = rainfall_mean, y = growth_pct)) +
  geom_point(alpha = 0.7) +
  geom_smooth(method = "lm", se = TRUE) +
  labs(title = "City Growth vs Mean Rainfall (2011–2017)", x = "Mean rainfall", y = "Population growth (%)")

p2 <- ggplot(joined, aes(x = temperature_mean, y = growth_pct)) +
  geom_point(alpha = 0.7) +
  geom_smooth(method = "lm", se = TRUE) +
  labs(title = "City Growth vs Mean Temperature (2011–2017)", x = "Mean temperature", y = "Population growth (%)")

ggsave("outputs/growth_vs_rainfall.png", p1, width = 8, height = 5)
ggsave("outputs/growth_vs_temperature.png", p2, width = 8, height = 5)

if (!all(is.na(joined$humidity_mean))) {
  p3 <- ggplot(joined, aes(x = humidity_mean, y = growth_pct)) +
    geom_point(alpha = 0.7) +
    geom_smooth(method = "lm", se = TRUE) +
    labs(title = "City Growth vs Mean Humidity (2011–2017)", x = "Mean humidity", y = "Population growth (%)")
  ggsave("outputs/growth_vs_humidity.png", p3, width = 8, height = 5)
}

message("Done. Outputs written to ./outputs/")
