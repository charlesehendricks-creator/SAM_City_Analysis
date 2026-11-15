# SAM_City_Analysis
Analyzed Budget and Population for 250 largest cities in US (2026)
US Cities 2026: ETL and Descriptive Statistics

An end-to-end data project that takes one messy raw text file of the 250 largest US cities, cleans it with Python and pandas, describes it with statistics and charts, and finishes with a Power BI dashboard.

Built by Charles Hendricks as the final project for a data analytics class (US Cities 2026 ETL and Descriptive Statistics Lab, instructor Kelvin Sparks).

The question

Counting cities is only one way to measure political control. This project asks: who really controls the budget and the population? It compares three ways of counting, by number of cities, by residents, and by dollars, and checks whether the answer changes when you account for tiers of city size, outliers, and New York City.

The data

US_cities_2026.txt is a pipe-delimited text file with 250 rows and 11 columns: city and state, population, city budget, mayor, mayor gender, term dates, form of government, and two extra columns (foo1, foo2).

The raw file is not analysis-ready. Quoted text, stray spaces, combined fields (city and state together, mayor name with birth year and party), and numbers stored as text all had to be dealt with first.

Pipeline

1. Extract

Load the raw file with pandas.read_csv using the pipe separator and the single-quote character.
Inspect shape, data types and the first rows, and document each formatting problem.

2. Transform

Strip stray spaces and quote marks from text columns.
Split State_City into City and State, and assign each state to a US Census Region.
Use regular expressions to split the mayor field into Mayor_Name, Birth_Year (blank when unknown) and Party (D, R, I, NP).
Convert Population and Budget to numbers and calculate Budget_per_Capita.
Validate: 250 rows, no duplicate Order, no missing values in key columns, every row has a party and region, count of mayors with no birth year, and a spot check of one city against the raw file.

3. Load

Save the cleaned table as us_cities_clean.csv.
Read it back with an explicit dtype dictionary and confirm the rows, columns and types match the original DataFrame.
Load the table into a SQLite database and answer a question with SQL (total population by region).

The cleaned table has 250 rows and 19 columns.

Analysis
Central tendency: mean, median and mode of population, budget and budget per capita, plus the mode of each categorical column. Continuous measures are binned first because a mode only makes sense for repeated values.
Charts: a donut chart of party, bar charts of government type and region, and histograms with mean and median lines.
Shape: skewness and excess kurtosis for each measure. A log10 transformation of population makes a heavily right-skewed distribution far more symmetric.
Empirical rule: the percent of cities within 1, 2 and 3 standard deviations, compared with the expected 68%, 95% and 99.7%.
Z-scores and scaling: z-scores, min-max scaling, and robust scaling, with a comparison table for selected cities.
Outliers: removal by the |z| > 3 rule and by the 1.5×IQR rule, with before-and-after statistics, box plots and histograms. The masking effect of extreme values on z-scores is discussed in the notebook.
Relationship: population against budget on log scales with a trend line, and the correlation with and without New York City.
Tiers and control projects
Five ordered tiers (Major, Large, Medium, Small, Tiny) for both City_Size (by population) and Budget_Size (by budget), with a cross-tab showing which cities rank higher or lower on budget than on population. A second version of city size uses equal-count quintiles.
Bloc column: Independent and Nonpartisan mayors are grouped with the Democratic caucus, giving a Democratic caucus bloc and a Republican bloc. The original Party column is kept to show both views.
Project A: each bloc's share of cities, population and budget, in 100% stacked bar charts, with and without the caucus rule.
Project B: the same shares within each city-size tier.
Project C: the same comparison without New York, then without the outliers.
Project D: mean and median budget per capita by bloc, with box plots.
Project E: a further breakdown beyond party (gender).
Write-up: a paragraph answering the main question using numbers from the projects.
Power BI

The .pbix file loads us_cities_clean.csv and uses Python visuals driven by pandas and matplotlib, with a slicer for filtering. Tier columns are sorted Major to Tiny with Power BI's Sort by column. Customizations I made to the provided scripts are described in the notebook.

Repository contents
File	Description
US_cities_2026.txt	Raw data
Notebook (.ipynb)	The full ETL, analysis, projects and write-up
us_cities_clean.csv	Cleaned table
us_cities_with_stats.csv	Cleaned table plus z-score and scaled columns
us_cities_no_outliers.csv	Outlier-free table
Power BI file (.pbix)	Dashboard with Python visuals
How to run
Open the notebook in Google Colab or Jupyter.
Place US_cities_2026.txt where the first code cell expects it (in Colab, /content/sample_data/).
Choose Runtime > Run all. The final cells write the CSV files and run the validation checks silently if everything passes.

Requirements: Python 3, pandas, numpy, matplotlib, scipy. SQLite support is built into Python.

Tools used

Python, pandas, NumPy, matplotlib, SciPy, SQLite, Jupyter/Colab, Power BI Desktop.

Notes

The assignment instructions and starter scripts belong to the course instructor. This repository contains my own solution and analysis.
