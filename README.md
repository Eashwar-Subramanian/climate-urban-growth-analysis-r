# Climate-Driven-Urban-Growth-Analysis-Using-R

---

## 🎯 Objective

To investigate whether climatic variables such as rainfall, temperature, and humidity have influenced population growth across Australian cities between 2011 and 2017.

---

## 🧰 Tools & Technologies

| Tool | Purpose |
|------|---------|
| **RStudio** | Data analysis and visualization |
| **R Packages** | `readxl`, `dplyr`, `tidyr`, `ggplot2` |
| **Data Wrangling Techniques** | `pivot_longer()`, merging, type conversion |
| **Imputation** | Median imputation for missing values |
| **Outlier Handling** | Winsorization |
| **Visualization** | Correlation plots, trend graphs using `ggplot2` |

---

## 🗂️ Project Workflow

### 1. 📥 Data Collection
- Meteorological and population data sourced from **Kaggle**.
- Weather data includes daily records of Australian cities.
- Population data covers census values for **2011 and 2017**.

### 2. 🧹 Data Preprocessing
- Removed irrelevant columns.
- Parsed and formatted the `Date` column.
- Missing values imputed using **median imputation**.
- Outliers handled using **Winsorization** to retain variability without distortion.

### 3. 📊 Data Transformation
- Used `pivot_longer()` to tidy the weather dataset.
- Merged 2011 and 2017 population columns into a single `Year` column for tidy structure.

### 4. 🔗 Merging Datasets
- Combined weather and population datasets based on `Location` and `Year`.

### 5. 🔄 Type Conversion
- Ensured proper data types:  
  - `numeric` for continuous values (e.g., rainfall, temperature).  
  - `factor` for categorical attributes (e.g., city names).

### 6. 🔎 Exploratory Data Analysis
- Created **time-series plots**, **correlation matrices**, and **scatterplots**.
- Observed population trends with respect to weather metrics across cities.

### 7. 📈 Statistical Insights
- Investigated the effect of **extreme weather patterns** on **urban growth**.
- Analyzed how **climatic anomalies** may correlate with **population shifts** or **growth stagnation**.

---

## 📌 Key Insights

- Preliminary results indicate a potential **correlation between extreme climate events** (e.g., high rainfall or heatwaves) and **migration or slowed growth** in certain regions.
- These insights can assist in:
  - **Urban planning**
  - **Disaster preparedness**
  - **Infrastructure investment**
  - **Climate change adaptation policies**

---

## 📚 Learning Outcomes

- Mastered data wrangling with `dplyr`, `tidyr`, and reshaping using `pivot_longer()`.
- Gained hands-on experience in handling **real-world data challenges** like missing values and outliers.
- Developed a structured approach to **merging multi-source datasets**.
- Drew meaningful **policy-relevant conclusions** from integrated datasets.

---

## 📖 References

- Australian Bureau of Meteorology (via Kaggle)
- Australian Bureau of Statistics
- R documentation for `tidyr`, `ggplot2`, and `dplyr`

---

## 📝 License

This project is for academic and non-commercial use only.
