# Climate-Driven Urban Growth Analysis Using R
**Investigating relationships between climate variables and Australian urban population changes (2011-2017)**

## 🎯 Research Question
Does climate (rainfall, temperature, humidity) influence population growth patterns across Australian cities between 2011 and 2017?

## 📊 Data Integration
**Meteorological Data:**
- Daily weather records for Australian cities
- Variables: rainfall, temperature, humidity measurements
- Source: Australian weather data via Kaggle

**Population Data:**
- Census data for 2011 and 2017
- City-level population figures
- Growth calculation between census periods

## 🔬 Methodology
**Data Preparation:**
- Missing value treatment using median imputation
- Outlier handling through Winsorization technique
- Data type standardization and format consistency

**Integration Process:**
- Temporal alignment of weather and population datasets
- Geographic matching of cities across both datasets
- Tidy data structure creation using pivot_longer()

**Statistical Analysis:**
- Correlation analysis between climate variables and population change
- Time-series exploration of weather patterns
- Population growth trend analysis by city

## 🛠️ Technical Implementation
**Language:** R  
**Data Wrangling:** dplyr, tidyr with pivot_longer() for data restructuring  
**Visualization:** ggplot2 for statistical charts and correlation plots  
**Analysis:** Statistical correlation and trend analysis techniques

## 📈 Analytical Outputs
- **Correlation Matrices:** Climate vs population growth relationships
- **Time-Series Plots:** Weather pattern visualization over study period  
- **Scatterplot Analysis:** Population change vs climate variable relationships
- **Trend Visualization:** Urban growth patterns with climate overlay

## 🌏 Australian Context
- **Geographic Scope:** Major Australian cities with available climate and population data
- **Temporal Range:** 2011-2017 for consistent census comparison
- **Climate Focus:** Key variables relevant to livability and urban planning

## 🎓 Research Value
- **Urban Planning:** Understanding climate influence on city growth
- **Policy Insights:** Data-driven approach to climate-population relationships
- **Methodology:** Reproducible framework for similar analyses
- **R Skills:** Advanced data manipulation and statistical visualization techniques

## 📁 Technical Skills Showcased
- Multi-source dataset integration
- Advanced R data wrangling (pivot_longer, joins)
- Statistical correlation analysis
- Climate data processing and visualization
- Population trend analysis techniques
