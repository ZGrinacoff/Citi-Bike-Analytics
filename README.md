# Citi Bike Analytics
Since 2013, the Citi Bike Program has implemented a robust infrastructure for collecting data on the program's utilization. Through the team's efforts, each month bike data is collected, organized, and made public on the [Citi Bike Data](https://www.citibikenyc.com/system-data) webpage. City officials have a number of questions on the program, so my task is to build a set of data reports to provide the answers.

All Citi Bike Trip History data was collected for these reports as CSV files. The timeline used for this analysis is January 2018 - September 2019. The following steps outline the Data Extraction, Transformation and Loading procedures and tools utilized in this project:

* Each CSV file located on the Citi Bike Data Webpage pertains to a particular month/year. Each file was downloaded and saved locally for timeline.

* Each file was cleanly formatted which allowed for easy loading into newly created PostgreSQL Database. 

* A new table was created with an ID added for each row (trip) of data.
      
    * Dates and Times for the "starttime" and "endtime" were stored as timestamps and set to EST timezone.

* Once Database and table were set-up and completed, it was now time to access PostgreSQL Server through Tableau to create visualizations and perform analysis.

### Additional Transformation for Trip Map
One of the tasks that I wanted to complete for my analysis was a map (spider map) that showcased each trip with a line and dots which would represent a trips start and end points, and would include additional information so that each trip could be clearly visualized. However, additional transformation of the data table was required to complete this endeavor so that each trip would have two rows that separate starting and ending geo-data, and a value that represented whether the row corresponded to the start or end of trip. These steps of which are outlined below:

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

### Analysis:
