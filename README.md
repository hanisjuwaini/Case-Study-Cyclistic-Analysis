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

The data is a public data obtained from Motivate International Inc. under this licence *https://ride.divvybikes.com/data-license-agreement*
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
- start_day: which days (Monday, Tuesday etc) the ride started at
- end_day: which days the trip ended at
- trip_duration_minute: total duration of the trip in minutes
- start_hour: the starting time of the trip (8:00,9:00,10:00 etc)
6. I did a quick check of the data after the steps above, where I calculated for the average of the trip duration.
![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/24ae38f7-738d-47f3-8171-04b896af9ce3)

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

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/dc88e990-9595-405c-8ac5-f5d798c12fe5)


Starting off with the main information, we have a total of 4,369,136 users, the members dominating with 59.76% (2,611,119) and 40.24% (1,758,017) of casual users. 
Now let's dive in deeper to see at which months do we get high trips.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/d775c7dd-75e8-4b5d-9147-ddf167161fd6)

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/71520635-5010-40b4-9a24-fa100cbda5e2)

From April till July, there is an increase in users, which then declines but remains comparatively higher than previous months. Those months are in the Spring and Summer seasons which make sense for the high numbers given that the weather is favourable for bike riding. The school summer break might be another factor in the peak months (June and July), where more families and kids are out to explore leisurely.
It's also can be seen whereas casual riders' usage tends to peak on the weekends and decline during the week, members' usage is constant throughout the whole week. We could assume that the members are using the bike to commute to their workplace or school, while the casual riders are using it for recreational purpose.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/79890f9d-1ee9-4948-8bfe-5825f054427e)

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/b89ad155-fabb-42c7-922d-05465fdfa919)

Based on the charts above, it is not surprising that members are spending less time on bike, with an average of <15 mins daily, as I assumed that they are using it as their transportation to their workplace which is accessible by bikes. While casual riders are spending almost 2x more than members for leisure purpose. 

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/ad6672ce-990e-4cd6-9c62-a2c04edbfc7f)

If we look at the peak hour based on the chart above, we can see that the number surges at early in the morning, 6am to 8am where the working hours for most business begin. And the following peak hour is between 4pm to 6pm where people are commuting back home.

Now let's see what type of bikes do the user prefer. Based on the chart below, the members choose the classic bike over the electric bikes, with no usage at all for the docked bike. Casual riders also seem to prefer classic bike over the others and followed by electric bike as their second preference.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/0ee39120-a103-4614-8669-29e6b74eefb9)

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/9a567249-3293-44ea-9aa8-b4c4bc2da27f)

From the charts below, I can see that the famous stations where most people picked up the ride are almost identical, such as DuSable Lake Shore Dr & Monroe St, DuSable Lake Shore Dr & North Blvd, 
Streeter Dr & Grand and Michigan Ave & Oak St. After further research these stations are apparently along the sea, surrounded by restaurants, pubs, playground and many more. 

 
![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/eacacd51-27c8-4443-8788-4c1fdf55f470)


![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/5b3a75b0-a344-4fdf-8c00-98db4b9b1678)

## Summary

Annual members are consistently using the bike for a brief period of time while casual rider spent more time on the ride especially on the weekend, despite using it inconsistently. The summer season has the highest riders meanwhile winter has the least.Also, both users have different preference of bike type according to their purposes at the moment. We could finally conclude that the casual riders are using the app for recreational purpose while the members mostly use them as their own transportation.


## Recommendation
1) Perhaps special package could be made if they are converting to annual member subscription after they hit certain subscriptions.
2) More offers should be done during the weekend, espicially on the peak hours (4pm-8pm). A new package which include only using the bike during weekend could be beneficial.
3) Do more promotion during the the spring and summer seasons.
4) Create more outdoor ads such to be displayed at the high traffic stations. Other option would be creating more digital marketing to be blasted on the social media. Make sure to highlight the offer and compare the normal price to the subscription price.

## Improving the Analysis

To improve this analysis I would be using the median instead of the average, which includes the outliers. Perhaps can try to use any statistical test to deal with the data to get more accurate results. 
Also, tracking the route of the users would definitely give more insights especially to see the difference between casual riders and members.



