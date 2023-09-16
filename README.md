# CS-2316-Final-Project
This is a data analytics project I did in Fall 2021 about correlations between teen pregnancy, crime rates, and dropout rates in python.

We answered the question: what is the relationship between crime, education, and teen pregnancies in Georgia?

## Our Hypothesis
As crime increases, education benchmarks decrease, measured using school attendance, HOPE eligibility, high school completers, and dropout rates, teen pregnancies increase.

## Data Collection
- **Downloaded Dataset #1:** 10 years of education drom Georgia Governor's Office of Student Acheivement
- **Downloaded Dataset #2:** 9 years of population data from US Census Bureau, 2020 population data converted from a PDF from Georgia General Assembly
- **Downloaded Dataset #3:** Shapefile from US Census Bureau
- **Web Collection #1:** Scraped data from Georgia Department of Public Health's Online Analytical Statistical Information System, HTML
- **Web Collection #2:** FBI Crime Data API with agency listing and agency offenses summary endpoints, JSON based API

## Data Cleaning
- Collecting education datasets into one DataFrame + related inconsistencies
- Column names were tuples in the teen pregnancy data
- Complex nested dictionary to DataFrame
- Formatting datasets to have the counties as the index and the years as the column names
- Reading to SQL + Excel

We had to use various methods to clean the data from dropping columns to reformatting the DataFrame. The education data needed to be concatenated into a larger DataFrame, then cleaned from 50,000 rows to 10,000 rows.

## Insights
- Insight #1: Counties with the highest crime and teen pregnancy counts
- Insight #2: Examining trends in education statistics
- Insight #3: Correlating teen pregnancy with school attendance
