# Citi Bike Analytics
Since 2013, the Citi Bike Program has implemented a robust infrastructure for collecting data on the program's utilization. Through the team's efforts, each month bike data is collected, organized, and made public on the [Citi Bike Data](https://www.citibikenyc.com/system-data) webpage. City officials have a number of questions on the program, so my task is to build a set of data reports to provide the answers.

All Citi Bike Trip History data was collected for these reports as CSV files. The timeline used for this analysis is January 2018 - September 2019. The following steps outline the Data Extraction, Transformation and Loading procedures and tools utilized in this project:

* Each CSV file located on the Citi Bike Data Webpage pertains to a particular month/year. Each file was downloaded and saved locally for timeline.

* Each file was cleanly formatted which allowed for easy loading into newly created PostgreSQL Database. 

* A new table was created with an ID added for each row (trip) of data.
      
    * Dates and Times for the "starttime" and "endtime" were stored as timestamps and set to EST timezone.

* Once Database and table were set-up and completed, it was now time to access PostgreSQL Server through Tableau to create visualizations and perform analysis.

### Additional Transformation for Trip Map
One of the tasks that I wanted to complete for my analysis was a map (spider map) that showcased each trip with a line and dots which would represent a trips start and end points, and would include additional information so that each trip could be clearly visualized. However, additional transformation of the data table was required to complete this endeavor so that each trip would have two rows that separate starting and ending geo-data, and a value that represented whether the row corresponded to the start or end of trip. The steps of which are outlined below:

* The original data table was accessed by Python script in Jupyter Notebook (trip_geo_data_transformation.ipynb) via connection to PostgreSQL Database which utilized the psycopg2 library and Pandas DataFrame.

* A SQL Query was executed with a date filter for all values between April, 1, 2018 - September 30, 2019 so as to limit the amount of data utilized for the visualization. My concern was that too large a data set would be less efficient to manage, and more difficult to visualize.

* After SQL query was executed, the entirity of the table was stored in a Pandas DataFrame, columns updated, and length of table checked against SQL query in PostgreSQL to ensure that no data was lost in process.

* Two new DataFrames were created for both the trip start and end data, with columns being dropped for irrelevent information.

* Column names for both DataFrames were renamed to be congruent.

* A new column named "path_id" was added to both with a value of "1" and "2" for trip start and end, respectively.

* The two DataFrames were then appended to create one large DataFrame with all data and criteria outlined above.

* Then, sorted by first "id", then "path_id" to ensure successful transformation.

* Finally, the new DataFrame was exported to a CSV file, without index.

* A new table was created in PostgreSQL Database, and CSV file was imported into server so that it may be accessed via Tableau for visualization of start and end trips.

### Analysis and Conclusions:

* How many trips have been recorded total during the chosen period and by what percentage has total ridership grown?
      
      * There were at total of 666,061 trips taken during the January 2018 - September 2019 time period. In 2018, 353,835 total trips were taken, while thus far (up to September 2019) in 2019 312,226 total trips have been taken. In the same time period during 2018, only 269,244 trips were taken. Therefore, 2019 has seen a 15.73% increase in trips taken.
      
* How has the proportion of short-term customers and annual subscribers changed?

      * It appears that the share of short-term customers and annual subscribers tends to fluctuate based on the time of year and season. Although subscribers make up the largest share of users, during the Spring and Summer the total number of short-term customers tends to increase. During January, February and March subscribers make up roughly 98% of total users. However, perhaps as the temperature and weather improves, the total share of customers increases, and the share of subscribers drops slightly. It appears that outreach is critically important during warmer months to optimize the increase in short-term customers.
      
* What are the peak hours in which bikes are used during summer and winter months?

      * Although peak hours for bike usage during the winter and summer are both in the mid-afternoon. There does appear to be a very slight difference by only one hour. Peak hours over the summer are typically at 3 PM, while peak hours during the winter tend to be at 4 PM. This is a very slight difference.
      
* Today, what are the top 10 stations in the city for starting and ending a journey?

      * Many of the same stations are the most popular to start and end a journey. Interestingly, these stations all tend to be west of the Hudson River, in New Jersey. Since 2013, the area has added 22 miles of new bike lanes, and with only one available crossing into Manhattan by bike, the George Washington Bridge, Citi Bike does try to restrict crossings into Manhattan. This is likely due to concern that bikes may not be returned to stations in New Jersey, which would negatively affect total bike availability in the area. Popularity of bike sharing has taken off in New Jersey since its inception and should continue to be highlighted and used as an example for other areas. 
      
* How does the number of total trips and trip duration change by age?

      * Age does appear to have an impact on the total number of trips and their duration. Individuals in their late 20's and early 30's tend to take the most and longest trips. However, one phenomena that I discovered during my analysis is that 50 year olds actually have the highest and longest number of trips. There's a sharp increase in total customers at this age that I find curious. Due to the duration and number of trips, this value is clearly an outlier but is it due to actual involvement in the bike share program, or perhaps were there issues for these customers. This should be addressed and further analysis should be required to determine the exact cause.
      
* Today, what is the gender breakdown of active participants (Male v. Female)? How effective has gender outreach been in increasing female ridership over the timespan?

      * Although there does appear to be some improvements in the increase of female ridership over time, males still make up the largest share of users. Further progress could be made to ensure that active female participants and total ridership between both genders continues to increase. The share of female bike riders across the country tends to be lower than the share of men. This is a problem that has clearly influenced female ridership for Citi Bike, and may continue unless steps are taken to address the issue. Safer bike lanes, lighter bike weight, and more convenient trip times will help to increase female ridership.
      
* Which bikes (by ID) are most likely due for repair or inspection in the timespan?

      * Currently, there are quite a few bikes that are likely due for repair and inspection. Based on total time duration over the 21 month period, there are 24 bikes that stand out. The bike ids and total trip duration are as follows:
      

|Bikeid	|     SUM(Tripduration)|
|-------------    |     -------------|
|29674	|1,991,778|
|29647	|1,565,923|
|29551	|1,939,385|
|29477	|1,498,153|
|29474	|1,457,693|
|29459	|2,265,030|
|29445	|1,620,008|
|29438	|1,962,020|
|29437	|1,945,392|
|29299	|2,807,762|
|29290	|1,566,151|
|29252	|2,220,189|
|29224	|2,059,412|
|29204	|1,462,680|
|29201	|1,544,599|
|26293	|1,677,009|
|26262	|1,913,406|
|26211	|1,710,101|
|26210	|1,736,669|
|26202	|1,730,192|
|26197	|1,434,758|
      
