# Case-Study-Cyclistic-Bike-Share-Analysis
A case study that is included in Google Data Analytics Certificate.

## Introduction

Cyclistic is a bike-share company in Chicago that has been launched since 2016. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

The marketing team is trying to maximize the number of annual memberships. Hence, an analysis needs to be done to understand how the members and casual riders use Cyclistic bikes differently so that recommendation could be done to convert the casual riders into annual members. As a junior data analyst in their company, I am responsible to derive insights, and to come up with solutions to this problem. 

## Business Task

The main problem is trying to **convert the casual riders into annual members on Cyclistic program.**
Three questions to be solved:
1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

## Data Preparation

The data is a public data obtained from Motivate International Inc. under this licence *https://ride.divvybikes.com/data-license-agreement*.
It is updated on monthly basis by the company itself. Since it is a first-hand collected data, it is considered as credible.

Link to the data: *https://divvy-tripdata.s3.amazonaws.com/index.html*

For the purpose of this analysis, data from **01 January 2022** until **31 December 2022** (12 months) was used.

For each of the files, it contains columns as below:
1) ride_id: Each user has their own unique IDs
2) rideable_type: Type of bikes used by the rider
3) started_at: Date and time where the ride started at
4) ended_a: Date and time where the ride ended at
5) start_station_name: Name of the station the ride started at
6) start_station_id: ID of the station the ride started at
7) end_station_name: Name of the station the ride ended at
8) end_station_id: ID of the station the ride ended at
9) start_lat: Latitude of the starting station
10) start_lng: Longitude of the starting station
11) end_lat:  Latitude of the ending station
12) end_lng: Longitude of the ending station
13) member_casual: The status of the user


## Data Cleaning

Since my Excel couldn't handle too much data, I opted for **Microsoft SQL** to do the data cleaning.
The code can be accessed here: 

1. After I uploaded the files on MSSQL using the export data tools, I straight away combined all the files into one big file using *JOIN* to make it easier.
2. I checked and removed the duplicates. 
3. Next, I used the *Trim* function to remove the leading spaces at the beginning and ending of sentences.
4. I noticed that there a lot of blank cells, hence I filtered and removed all the incomplete data.
5. Then, I added on new columns:
- day_of_week which days (Monday, Tuesday etc) the ride started at
- ride_length: total duration of the trip in minutes
- starting_hour: the starting time of the trip (8:00,9:00,10:00 etc)
```
-- Formula used to create new info
UPDATE  all_year
SET day_of_week = DATENAME(weekday,started_at),
    month = DATEPART(month,started_at),
	ride_length = DATEDIFF(minute,started_at,ended_at)
 
```
6. I did a quick check of the data after the steps above, where I calculated for the average of the trip duration.

```
SELECT MIN(ride_length), MAX(ride_length), AVG(ride_length)
FROM all_year

```

 Then I noticed that the average is 17 minutes, despite the maximum minutes being 34,355 minutes which is approximately 23 days. After further checking, there were around 155 rows with the same situations,  
 which the trip duration is more than 1 day. 
 To make the analysis easier for this case, I decided to removed all the trips that are more than 1 day. The trips could be a result of a system error and such.
 
 ## Data Analyzing
 
Now, the data is cleaned and ready to be analyzed more thoroughly. I will continue using **MSSQL** to break down the data into few tables. This will make it easier for me to do the visualisation later on.
 
I started the analysis by calculating how many users are currently using the apps.

```
SELECT member_casual, COUNT(ride_id) AS total_trip
FROM all_year
GROUP BY member_casual
ORDER BY total_trip DESC
```

Then, I did the same calculation to find the number of trips for each months to see the trend in 2022.

```
SELECT member_casual, month, COUNT(ride_id) AS total_trip
FROM all_year
GROUP BY member_casual, month
ORDER BY member_casual, total_trip DESC, month

```

Next, I calculated the number of trips for each days in a week

```
SELECT member_casual, day_of_week, COUNT(ride_id) AS total_trip
FROM all_year
GROUP BY member_casual, day_of_week
ORDER BY member_casual, total_trip DESC, day_of_week

```

Then I calculated the average trip duration for the monthly and daily data.

```
SELECT member_casual, month, AVG(ride_length) AS average_trip
FROM all_year
GROUP BY member_casual, month
ORDER BY member_casual,  average_trip DESC, month

```
```

SELECT member_casual, day_of_week, AVG(ride_length) AS average_trip
FROM all_year
GROUP BY member_casual, day_of_week
ORDER BY member_casual,  average_trip DESC, day_of_week

```

To find the peak hours, I create a new table focusing on the starting hour column.

```
SELECT member_casual, starting_hour, COUNT(ride_id) as total_trip
FROM all_year
GROUP BY member_casual, starting_hour
ORDER BY member_casual, starting_hour 
```

 
Then I created two tables, one for the start station, where the users picked up the bike and another is  for the stations where they returned the bike.
 
 ```
 SELECT start_station_name, count(*) AS total_user
FROM all_year
GROUP BY start_station_name
ORDER BY  total_user DESC, start_station_name


SELECT end_station_name, count(*) AS total_user
FROM all_year
GROUP BY end_station_name
ORDER BY  total_user DESC, end_station_name
 ```


Lastly I created a table to see if the users have any preferences on the bike type.

```

SELECT rideable_type, member_casual, COUNT(ride_id)
FROM  all_year
GROUP BY rideable_type, member_casual
ORDER BY rideable_type, member_casual

```


## Data Visualisation

I saved all the results in xls. format to create charts on Excel for visualisation purpose.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Analysis/assets/87611715/13d099ba-20ba-4dad-a4b7-d44c54535903)


Starting off with the main information, we have a total of 4,369,136 users, the members dominating with 59.76% (2,611,119) and 40.24% (1,758,017) of casual users. 
Now let's dive in deeper to see at which months do we get high trips.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Analysis/assets/87611715/b7073e91-bb01-4b24-8b0a-136fbc202825)

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Analysis/assets/87611715/7927ee71-8593-411d-8439-ab75daad1ac3)


From April till July, there is an increase in users, which then declines but remains comparatively higher than previous months. These months correspond to the Spring and Summer seasons, which make sense for the high numbers given that the weather is favourable for bike riding. The school summer break might be another factor in the peak months (June and July), where more families and kids are out to explore leisurely. 
It's also can be seen whereas casual riders' usage tends to peak on the weekends and decline during the week, members' usage is constant throughout the whole week. We could assume that the members are using the bike to commute to their workplace or school, while the casual riders are using it for recreational activities.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Analysis/assets/87611715/51840be2-7030-4777-a4b1-6fc77dc91c6a)

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Analysis/assets/87611715/d0219ce1-b5b6-4cc6-b965-4078450e65c8)


Based on the charts above, it is not surprising that members are spending less time on bike, with an average of <15 mins daily, as I assumed that they are using it as their transportation to their workplace which is accessible by bikes. While casual riders are spending almost 2x more than members for leisure purpose. 

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Analysis/assets/87611715/68d5e729-8368-4791-98a5-b458b4f26a7f)

If we look at the peak hour based on the chart above, we can see that the number surges at early in the morning, 6am to 8am where the working hours for most business begin. And the following peak hour is between 4pm to 6pm where people are commuting back home.


Now let's see what type of bikes do the user prefer. Based on the chart below, the members choose the classic bike over the electric bikes, with no usage at all for the docked bike. Casual riders also seem to prefer classic bike over the others and followed by electric bike as their second preference.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Analysis/assets/87611715/0aae549e-8382-411d-a877-6381afbe39a5)

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Analysis/assets/87611715/adb2a519-3e74-458c-8b82-8f4e0174c59a)


From the charts below, I can see that the famous stations where most people picked up the ride are almost identical, such as DuSable Lake Shore Dr & Monroe St, DuSable Lake Shore Dr & North Blvd, 
Streeter Dr & Grand and Michigan Ave & Oak St. After further research these stations are apparently along the sea, surrounded by restaurants, pubs, playground and many more. 

 
![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Analysis/assets/87611715/d0d2b8c0-2fea-483b-8a8e-58dfa1e8a460)

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Analysis/assets/87611715/22b8f55d-c76c-4a01-8ddb-470ab796f610)


## Summary

- User patterns: Annual members predominantly used the bike sharing app on weekdays, indicating that they primarily utilized the service for their daily commute to work. In contrast, casual users were more likely to use the app during weekends, suggesting a preference for recreational activities. Also, both users have different preference of bike type according to their purposes at the moment.
- Ride length: Annual members are consistently using the bike for a brief period of time while casual rider spent more time on the ride especially on the weekend, despite only using it occasionally.
- Seasonal variation: During summer season both members and casual users showed an increase in ride frequency, possibly due to favorable weather conditions.


## Recommendation
1) Create a special package for users who switch to an annual membership subscription after reaching a certain number of rides.
2) Increase promotional offers specifically for weekends, especially during peak hours from 4pm to 8pm or introduce a new package exclusively for weekend rides.
3) Focus on increased promotional activities during the spring and summer seasons.
4) Create more outdoor advertisements at high-traffic stations. Additionally, explore digital marketing strategies on social media platforms, emphasizing the benefits of membership and highlighting the cost savings compared to regular pricing.

## Improving the Analysis

To enhance the accuracy of this analysis I would be using the median instead of the average, as it is less affected by outliers. Additionally, employing statistical tests to handle the data can provide more precise results. Furthermore, tracking the route taken by the users would definitely yield valuable insights particularly to see the difference between casual riders and members.



