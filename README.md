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

We used search engines to find datasets as well as Data.gov. We used selenium to scape, but we had some issues with selenium working. We had to insall chromedriver and geckodriver. Additionally, we had to download pdf for 2020 population data and convert to excel to then join with other population data from 2010-2019. We had to use geopandas to read the shapefile. We used the FBI documentation to determine endpoints of the FBI API. The requests took several hours to run until changing computer configuration.

## Data Cleaning
- Collecting education datasets into one DataFrame + related inconsistencies
- Column names were tuples in the teen pregnancy data
- Complex nested dictionary to dataframe
- Formatting datasets to have the counties as the index and the years as the column names
- Reading to SQL + Excel

We had to use various methods to clean the data from dropping columns to reformatting the dataframe. The education data needed to be concatenated into a larger DataFrame, then cleaned from 50,000 rows to 10,000 rows. We had to drop NaN rows, drop unneccessary columns we were ot planning on using, and make sure each column contained uniform data. Changed the column names in the teen pregnancy data to be strings, and renamed them. Converted a nested dictionary into a dataframe from the crime data. We had too format the education dataframes to match the teen pregnancy and crime dataframes by setting the index to the county names and the column names to the years. We read to SQL using the module <code>sqlalchemy</code>, and read to excel using the <code>to_excel</code> function in pandas and an excel writer object. We had to make the indicies uniform, and the same shape. We had to reset the index with parameter drop = True to use the <code>to_sql</code> method.

## Insights
### **Insight #1:** Counties with the highest crime and teen pregnancy counts.

We chose to find the counties with the highest numbers of teen pregnancies and crime, taking the highest twenty of each. To take the highest twenty counties, we looked at each individual year column and used <code>.nlargest()</code> to find the rows with the largest values simultaneously making them into smaller DataFrames. After that, we used <code>.concat()</code> to join all of them together into a DataFrame collection.

For us, it was interesting to see which counties made up the highest twenty and recognize factors (socioeconomic, geographic, and others) that influenced the rankings. 

One interesting observation was that Muscogee had crime numbers much greater than the bulk of Georgia counties for the majority of decade but dropped to 0 reported crime in 2020. 

### **Insight #2:** Examining trends in education statistics.

Within the education data we collected from Georgia, we had four different sections: student attendance, HOPE eligibility, high school completers, and dropout students. In order to get a better idea of how each section the education data looked throughout the years. We took the average of each section of data within education per year. We then graphed each part of education with a line plot. This allowed us to see major trends from the education graphs.

From the graphs, we noticed that within the attendance data there was a major drop in the number of students attending school in 2011-2012, but the trend was mostly positive except for the recent years. For HOPE eligibility the graph is trending positively but took a dip between 2014 and 2016. The amount of high school completers has had an overall positive trend throughout all the years except for a minor dip in 2016, with a drop in 2011-2012. The dropout graph had a spike in 2011-2012 then trended positively till a spike in 2017-2018, before decreasing in the recent years. There is a common trend of a decreased amount of student attendance, decreased amount of high school completers, and an increase of dropout students in 2011-2012. A possible reason for the decreases within the education data in 2011 could be due to states decreasing funding and promotion for higher education, Georgia decreased their funding by 11.9%. There were several tornado outbreaks within Georgia during 2011 period destroying homes and schools. Another trend we noticed in 2018-2020, more students are completing high school, there are less dropout, and more students are HOPE eligible.

### **Insight #3:** Correlating teen pregnancy with school attendance.

We have three topics: education, crime, and teen pregnancies. We decided that within education, we would use the attendance dataset to compare with the teen pregnancy dataset and the crime dataset. Within this insight we looked at the correlation between the percent of teen pregnancies per capita, and the percent of attendance per enrollment.

In order to use the seaborn plot to compare these two datasets, we had to make the datasets we wanted to compare. From the same source we got the attendance data, we had to pull the enrollment data and concatenate them to create a large DataFrame in order to divide them to get a percentage by county per year. We had to completely change how the DataFrame was ordered, using the counties as the indexes, and the columns were the years for the dataset to have the same shape as the teen pregnancy DataFrame in order to make a comparison. For the teen pregnancy DataFrame, we had to get population data per county from another web source and create a DataFrame from the population data that has the same shape as the teen pregnancy DataFrame in order to divide them to get a percentage per capita by county for each year. After we got the appropriate datasets, and they had the same shape, we were able to use the <code>.corr_with()</code> pandas function to get a correlation by year, and a correlation by county. In addition, we also used the <code>sns.regplot()</code> which plots the data with a linear regression. In order to get all the data per year we did a for loop to put each year on the graph.

As you can see from the seaborn plot above, there is a weak positive correlation between all counties by year. However, if you look within the DataFrames certain counties like Whitfield might have a strong correlation, which became an outlier within the overall plot. Also, compared to the attendance percentages, the teen pregnancies percentages were much lower because teen pregnancies are based on the overall population of the county while attendance is based on the number of students enrolled in a school by county per year. The positive correlation means the two variables work in tandem, as one increases, the other does. In other words, as attendance increases, the amount of teen pregnancies increases.

### **Insight 4:** Correlating crime with school attendance.

We have three topics: education, crime, and teen pregnancies. We decided that within education, we would use the attendance dataset to compare with the teen pregnancy dataset and the crime dataset. Within this insight we looked at the correlation between the percent of crime per capita, and the percent of attendance per enrollment. 

We followed the same process as in insight 3 but brought in another dataset. We used the previously made percentage attendance per county DataFrame. For the crime DataFrame, we had to get population data per county from another web source and create a DataFrame from the population data that has the same shape as the crime DataFrame in order to divide them to get a percentage per capita by county for each year. After we got the appropriate datasets, and they had the same shape, we were able to use the <code>.corr_with()</code> pandas function to get a correlation by year, and a correlation by county. In addition, we also used the <code>sns.regplot()</code> which plots the data with a linear regression. In order to get all the data per year we did a for loop to put each year on the graph.

As you can see from the seaborn plot above, there is a weak negative correlation between all counties by year. However, if you look within the DataFrames certain counties like Whitfield might have a stronger correlation, which became an outlier within the overall plot. Also, compared to the attendance percentages, the crime percentages were much lower because crime is based on the overall population of the county while attendance is based on the number of students enrolled in a school by county per year. In addition, there were NaN values within the DataFrames because there was an extremely low amount of crime per capita in some counties, therefore, when finding the correlation between the two frames it gave a NaN value. The negative correlation implies the relationship between the variables are inversely proportional.

For our fourth visualization, we used a seaborn regplot, regression plot, in order to plot a linear regression. It looks extremely complicated with the line for each year on the same graph, but we did that in order to make an overall conclusion about the correlations between attendance with crime, and attendance with teen pregnancies. We could identify that there is a weak positive correlation between percent attendance per enrollment, and percent teen pregnancies per capita. Therefore, as attendance increases, teen pregnancies increase. Also, we could identify that there is a weak negative correlation between percent attendance per enrollment and percent crime per capita. As crime increases, teen pregnancies decrease. Although it is hard to make this conclusion as they are weak correlations. We would need more data with stronger correlations in order to make a conclusion between the datasets.

### **Insight 5:** Observing trends with distributions.

For this insight, we observed per capita (for attendance per enrollment) distribution and trends among the Georgia Counties over the 2010-2020 decade. This insight provides important data on how Georgia counties are faring in reducing their teen pregnancy and crime rates and raising their attendance rates. 

Found per capita the case of the education attendance data, we utilized the school enrollment data and found their per enrollment by dividing the values in the attendance DataFrame by the enrollment values. With these datasets, we made a box and whiskers plot of each using matplotlib’s box plot.

### **Insight 6:** Mapping out crime and teen pregnancies per capita.

For this insight, we mapped out the counties to provide both a visualization and recognize the geographic feature of crime and teen pregnancy rates for their county. This insight takes the geographic factor of crime and teen pregnancy rates into account by looking at counties and their neighbors. We expected that geography influenced rates because of population patterns, and we also expected that neighboring counties would have similar rates.

To get the geographic data, we first read in a Shapefile of the Georgia counties with <code>geopandas</code>. Initially, we simply plot the per capita data onto their respective maps using <code>matplotlib</code> but only the counties with extreme values were noticeable. For comparison, we took the base ten logarithm on each value in each dataset then mapped those values.

To see a more direct correlation between crime and teen pregnancies we followed the same process from insight 3 and 4 and found that there is a weak positive correlation as we suspected.

## Overall Project Findings
- **Trends:**
  - Teen pregnancy, dropout rate is decreasing.
  - HOPE eligibility, graduation, attendance is increasing.
  - Crime has been relatively stable.
- **Results:**
  - Population density is a primary factor in crime and teen pregnancies.
  - There is no strong correlation between crime, education, and teen pregancies.
  - Crime, teen pregnancy, and education likely Pareto distributions.

Based on Insight 1 which shows the maximum counties for teen pregnancies and crime, Muscogee would need the most resources with crime however associated reporting bias should be considered. Overall crime per capita was lower than expected. There are a lot of factors that are shared in many issues related to our topics, socioeconomic factors, domestic factors in homes we cannot measure in data. We cannot conclude that there is a direct relation between all three topics; however, they share a lot of factors and related issues. The distribution showcased by the crime and education is likely a Pareto which relates to socio and geographic factors. Since our correlation plot does not have a strong relation, we cannot confidently conclude based on our data that crime, teen pregnancies, and education are related.

## Project Impact
We learned Selenium, Seaborn Plots, SQL alchemy, tqdm, GeoPandas, installation and command line functions, interpreting errors for our respective operating systems. We explored deaper into matplotlib, Pandas in gneeral, Beautiful Soup, and Requests. Our greatest challenge was finding appropriate datasets, and ways to read them in to clean them.
