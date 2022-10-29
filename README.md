# (Ford GoBike Data Exploration)
## by (Naomi Ogbechi)

## Dataset

The Ford GoBike system data includes information regarding about 180,000 individual rides made in a bike-sharing system covering the greater San Francisco Bay area in the United States, for the month of February, in the year, 2019. Bikeshare is designed for shorter duration trips. The bike-share system works by circulating—sharing—bikes between users. If a rider needs a bike for longer than 45 minutes, they can always check out a new bike mid-trip to complete their ride.

The Ford GoBike system data provides anonymized observations for rides including features such as the duration in seconds, the start and end station information, the user type; whether subscriber or customer, as well as some demographic information like birth year and gender. All data is based off free material from this [link](https://github.com/BetaNYC/Bike-Share-Data-Best-Practices/wiki/Bike-Share-Data-Systems) which was provided by Udacity.

The wrangling process included the following phases:
1. Gathering data:- This was the first step in the wrangling process. The 201902-fordgobike-tripdata.csv file was read using pandas nto our programming environment for the ‘assessing data’ phase.
 
2. Assessing data:- The goal of this step was to assess and detect issues in the gathered data. Upon assessing the data 
visually and programmatically, the following issues were found:
  - There are missing values in the data.
  - 'bike_id', 'start_station_id' and 'end_station_id' columns should be in category format.
  - We need to separate the 'start_time' and 'end_time' columns to contain only the dates and times.
  - We also need a column for day of week when rent occurred to facilitate analysis.
  - 'member_birth_year' should be in integer format not float.
  - We need a column for age to facilitate analysis.
  - 'user_type' and 'member_gender' should be of category data type.
  - We don't need all the columns in the data.

3. Cleaning data:- In this phase, the issues detected in the ‘assessing data’ phase were cleaned. I clean the issues detected with the following:
  - Drop rows with missing values.
  - Convert 'start_time' and 'end_time' columns to datetime data type using pd.to_datetime function. Create new columns; 'start_date' and 'end_date'.
  - Create start and end time bin columns using pandas pd.cut method.
  - Create a column named 'ride_day' with values as the day of the week when bike was rented for each ride and convert column to ordered category data type.
  - Update 'start_time' and 'end_time' columns to contain only the time and not the date using dt.time function.
  - Create new column 'duration_mins' with the 'duration_sec' column divided by 60.
  - Convert 'member_birth_year'column to integer data type. Define a function to calculate the age of each user as at 2019. Apply function to the data and create a new column called 'member_age'. Convert to integer data type.
  - Remove brackets from 'start_station_name' and 'end_station_name' columns using defined function.
  - Convert 'member_gender', 'user_type', 'bike_share_for_all_trip' and 'bike_id' columns to category data type.
  - Drop columns which won't be used for analysis.

4. Additional Wrangling Efforts:- In order to facilitate analysis, additional wrangling efforts were made and these included:
  - Subsetting the data to only include trips with 'member_age' less than 85.
  - Removing trips that exceeded 60 minutes from the data.
  - Identifying days that were holidays and filtering them out from the data.
  - Creating a new 'start_hour' column in the new filtered data with column values being the hour the trip was started. 
  - Creating a new dataframes to plot 2d bar charts
  - Subsetting the no_holiday data to select only the top 10 stations in the data.
  - Creating a new dataframe called ‘top_stations’ with the total number of trips for each of the top 10 stations, the station name, the latitude and the longitude.


## Summary of Findings

I started by exploring the dataset as a whole and looking at each feature independently. From this, I arrived at the following insights:

- There are 2 types of users in the dataset. They include Customers and Subscribers. Subscribers are residents with annual passes and Customers are visitors with 24-hour passes or three-day passes.[(Wheretraveler.com)](https://www.wheretraveler.com/san-francisco/play/guide-bay-area-bike-share) 
- Majority of trips were taken by Subscribers. Approximately 1 in every 10 trips, selected at random, was taken by a Customer.
- Majority of bike users are Male. 
- The distribution of age is skewed to the right, with a lot of users around the age range of 25 to 40. The age distribution is bimodal, with a peak between 25 and 30 and another between 30 and 35.
- 19% of trips occurred on a Thursday. Also trips on the weekend, i.e. Saturday and Sunday, have the least frequency. 
- Most users embarked on their trips in the afternoon within the time window of 15:00pm to 17:59pm. Also, night trips are less frequent. 
- Majority of trips last between the duration of 4-16 minutes and longer trips are also less frequent. This is in line with the purpose of the bike share system which is for short duration trips.
- The Powell St BART station is the most visited and most popuplar station for bike user trips. We can also see that the top 10 start stations and the top 10 end stations are the same albeit with some fluctuations in position.
- Bikeshare for All is a subsidized membership program which makes membership accessible to low-come individuals. It includes trips up to a full hour without redocking [(Sfmta.com)](https://www.sfmta.com/blog/bikeshare-pricing-frequently-asked-questions-faq). About 10% of trips were taken by users with the bikeshare for all membership. Users wih the bikeshare for all membership took very few trips.


I then tried to explore whether there were relationships in the data through bivariate and multivariate plots. Unfortunately, there were no strong relationships. However, I observed that there were clusters and patterns when the variables were plotted against each other. This helped to identify and develop a profile/ a persona for each of the users. The following insights were gotten:

- For both user types, Males exceed Females. 
- The days of the week with the highest trip activity for Subscribers and Customers respectively are Tuesday and Friday. There is increased ride activity during holidays.
- Both users mostly took trips at 8:00 (8am) and at 17:00 (5pm) in a day. The highest peak for subscribers and customers respectively is 8am and 5pm.
- There are only Subscribers with the bikeshareforall membership. The average duration of subscribers does not appear to be impacted by the bikeshareforall membership. Also, subscribers who are not bikeshareforall members tend to be older than those who are.
- Majority of trips for Subscribers fell between 4-10 minutes while that of Customers is 4-16 minutes.
- Majority of trips by both Subscribers and Customers were taken by users within the age range of 25 to 35. Subscribers aged 26 and 31 made the most trips, with age 31 having the largest frequency. For Customers, majority of trips tend to be made by users aged 30.
- The Powell St BART Station is most frequented by both Customers and Subscribers.
- The mean ages of the genders appear consistent across the board, with female customers and subscribers having a mean age of 33, and male customers and subscribers having a mean age of 34. For users with the gender type 'Other', subscribers appear to be older than customers on average, but only slightly. Hence we can infer from this that generally subscribers and consumers are middle-aged.
- For all gender types, customers take longer duration trips than subscribers.
- For every day of the week, customers generally engage in longer trips than subscribers. Customers and subscribers engage in longer trips on weekends (i.e. Saturday and Sunday) than they do on weekdays. The peak average duration for Customers is 18 minutes while that of Subscribers is 11 minutes.
- The stations in the San Francisco bay area consists of stations in the cities; Oakland, San Francisco and San Jose.
- The top stations which both customers and subscribers use are concentrated in the eastern part of San Francisco.


## Key Insights for Presentation 

For the presentation, I will focus on the notable characteristics i.e. similarities and differences I was able to extract from each feature with regards to the user_type variable. I start by introducing some information regarding the general characteristics of the data, then I introduce the different types of users in the data and introduce plots showing the similarities and differences between the users. 

Recall that there are 2 types of users in the dataset, which include Customers and Subscribers. From [(Wheretraveler.com)](https://www.wheretraveler.com/san-francisco/play/guide-bay-area-bike-share), I learnt that subscribers are residents with annual passes, while customers are visitors with 24-hour passes or three-day passes.

Using a pie chart, I explored the proportion of bike rides by subscribers to that of customers. After which, I checked the gender distribution of subscribers and customers using a bar chart.

In a bid to determine if there were peculiarities to subscribers and customers, I used an overlayed histogram plot to check the age distribution of customers and subscribers.

Furthermore, I used a clustered bar chart to check for trends in the frequency of rides on respective days of the week, I further screened out some days of the week to get a more accurate result.

After checking for trends in the days of the week, I decided to go granular and check the times of day in which most rides were undertaken by customers and subscribers. I did this using a clustered bar chart.

Next, I established the average duration of customer's and subscriber's bikes rides using a 2d bar chart.

In the next phase I explored the popularity of stations around the the San Francisco bay area using plotly's scatter mapbox plot. Then, I highlighted the locations of the most popular stations to observe any patterns.

Finally, Bikeshare for All is a subsidized membership program which makes membership accessible to low-come individuals. It includes trips up to a full hour without redocking [(Sfmta.com)](https://www.sfmta.com/blog/bikeshare-pricing-frequently-asked-questions-faq). Using a 2d bar chart, I decided to explore the usage of the Bikeshare program by subscribers and customers.

