# Climate-Driven-Urban-Growth-Analysis-Using-R

Objective
The objective of this project was to analyze weather patterns and their impact on population growth across Australian cities. By merging weather data (meteorological observations) with city population data from 2011 and 2017, the goal was to uncover potential correlations between extreme weather events and urban growth trends. This analysis could provide insights into how weather influences migration, infrastructure development, and urban planning.

Tools & Techniques Used
The primary tools and techniques used in this project included:

1. RStudio: For data pre-processing, manipulation, and analysis.
2. R Packages: readxl, dplyr, tidyr for data manipulation, and ggplot2 for data visualization.
3. Data Transformation: Pivoting data using pivot_longer() for tidying the datasets and ensuring they adhere to tidy data principles.
4. Data Imputation and Outlier Handling: Median imputation for missing values and Winsorization for outlier treatment.
5. Type Conversion: Converting columns into appropriate data types (numeric, factor).
6. Merging Datasets: Combining weather and population datasets based on common attributes like Location and Year.

This project involved the following key steps:

1. Data Collection and Import: Weather and city population datasets were sourced from Kaggle and imported into RStudio.
2. Data Cleaning: Non-relevant columns were removed, and the Date column was converted to a usable date format. Missing values were imputed, and outliers were handled using Winsorization.
3. Tidying and Reshaping: The weather dataset was pivoted to transform multiple columns into a unified structure for easier analysis. The city population dataset was reshaped to merge both years (2011 and 2017) into a single column.
4. Merging Datasets: Both datasets were merged by common variables such as Location and Year.
5. Data Type Conversion: Columns were converted to appropriate data types (e.g., numeric for continuous variables and factor for categorical ones).
6. Analysis and Exploration: Initial explorations and visualizations were performed to understand trends and relationships.

This project was chosen because it combines two highly relevant datasets—weather and population—which are critical for urban planning and climate change research. By understanding the relationship between weather patterns and population growth, the project can provide valuable insights into how climate variables affect urbanization, infrastructure, and public policy. It is an important step towards informed decision-making in areas such as disaster management, urban expansion, and migration patterns.

=The methodology followed in this project can be summarized in the following steps:

1. Data Collection: Acquiring datasets from Kaggle related to Australian weather and city populations.
2. Pre-processing: Cleaning, imputing missing values, removing irrelevant columns, and handling outliers.
3. Tidying Data: Reshaping datasets to adhere to tidy data principles using the pivot_longer() function.
4. Merging Data: Combining the weather and population data based on common attributes.
5. Type Conversion: Converting data columns into the appropriate types for analysis (e.g., numeric, factor).\
6. Exploratory Analysis: Initial data exploration to identify trends, correlations, and patterns between weather data and population growth.
7. Statistical Analysis: Conducting further analysis to draw insights and conclusions from the data.

In conclusion, this project effectively combined weather and population data to provide a deeper understanding of how weather patterns might influence population growth in Australian cities. By applying data cleaning, transformation, and analysis techniques, it was possible to uncover meaningful insights that can be used to inform urban planning and public policy. The project highlights the importance of merging different data sources and ensuring that data is tidied and structured correctly for meaningful analysis. The findings may be particularly useful for future studies on climate change, urbanization, and migration patterns.
