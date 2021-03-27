# ETL-Project

## Background
For this project, we were tasked with finding an interesting data source and performing the ETL process on it. In the below sections, you will read how we Extracted our data, made necessary transformations to it and loaded it into a database. 

## Extract
First, we explored kaggle.com to find an interesting data source to work with. We found our winner in a data source that provided 3 CSV files with the ranks of the top restuarants from 2020. We found this data interesting because not only do we love going out to eat, but also because of the specfics about the way each file was created, seen below.

Overview of each CSV file:
* Future 50:
    * Provided the rankings of the future 50 top restaurants. This provides a list of what are deemed the fastest growing restaurant with sales between $20 and $50 million. Rankings are based on the percent change in systemwide sales from 2018 to 2019.
* Independence 100:
    * Provided the rankings of the top 100 Independent Restaurants from 2020. An independent restaurant is defined as one that has no more than five locations. Rankings are based on gross 2019 food and beverage sales.
* Top 250:
    * Provided rankings of the top 250 restaurants from 2020. This provides a list of the top 250 chain restaurants based on company financial filings, direct operator surveying, franchise disclosure documents, and proprietary valuation algorithms.

* All data was pulled from Restaurant Business Online

After exploring this data, we downloaded it from kaggle, and loaded it into a Jupyter Notebook file. We chose this file type because we find it is easy to work with when performing ETL. 

The data described above can be found at https://www.kaggle.com/michau96/restaurant-business-rankings-2020.

## Transform
Once our data was loaded into our Jupyter Notebook, we got to work make transformations. We performed the following tranformations with our data:
1. Renamed all of the Sales columns to indicate that the units were in millions and fixed the Independence 100 dataframe, who's sales we not represented in millions so that there was consistency across the tables.
2. We dropped some extra columns that we deemed unusable for future analysis. We the "Content" and "Headquarters" columns from the top 250 dataframe as they contained a lot of NaN values and did not provide valuable information.
3. We grouped the independent 100 dataframe by Restaurant to combine any duplicated within the table. We summed the "Sales in Millions" and "Meals Served" columns, averaged the "Average Check" column and provided a count of each restaurant so the viewer can see if it was represented in the original table multiple times. Finally, we sorted this new dataframe by Sales in Millions in descending order to see the new highest ranking Restaurants.
4. To maintain the integrity of our Future 50 dataframe, we made a copy. Our objective was to merge the dataframe with the Independent 100 list based on the location details. 
5. Using the copied dataframe, we separated the "Location" values into two separate columns ("City" and "State") and stored them in a new dataframe. 
6. To add the "City" and "State" data to our original copy of the Future 50 dataframe, we created the new columns then inserted the values split out from our previous step. Finally, we dropped unnecessary columns.
7. To maintain the integrity of our Independent 100 dataframe, we made a copy then dropped unnecessary columns. We decided to make both Future 50 and Independent 100 copied dataframes have matching columns to combine them easily. We finally concatenated the two dataframes to make a Top 150 list. 
8. We took the Top 150 dataframe and sorted the rows by Sales in Millions. Next, we reset the index and made it count from 1 to show the new rankings. Finally, we renamed the index to Rank and dropped unnecessary columns.


## Load
After we were satisfied with our data transformations, we used the following procedure to load our dataframes into our Postgres Database:
1. We created a python file to hold our password. Using the password file, we created an engine to connect to our Postgres database and confirmed a successful connection by checking for existing tables.
2. We preceded to add each of our transformed dataframes to our Postgres database. To validate the data was inserted correctly, we ran a simple SQL query. 






