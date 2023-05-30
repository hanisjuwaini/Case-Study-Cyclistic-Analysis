# Case-Study-Cyclistic-Bike-Share-Analysis
A case study that is included in Google Data Analytics Certificate.

## Introduction

Cyclistic is a bike-share company in Chicago that has been launched since 2016. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and
returned to any other station in the system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

The marketing team is trying to maximize the number of annual memberships. Hence, an analysis needs to be done to understand how the annual members and casual members use Cyclistic bikes differently so that recommendation could be done to convert the casual riders into annual members. As a junior data analyst in their company, I am responsible to derive insights, and to come up with solutions to this problem. 

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
    start_day: which days (Monday, Tuesday etc) the ride started at
    end_day: which days the trip ended at
    trip_duration_minute: total duration of the trip in minutes
    start_hour: the starting time of the trip (8:00,9:00,10:00 etc)
5. I did a quick check of the data after the steps above, where I calculated for the average of the trip duration.
![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/24ae38f7-738d-47f3-8171-04b896af9ce3)

 Then I noticed that the average is 17 minutes, despite the maximum minutes being 34,355 minutes which is approximately 23 days. After further checking, there were   
 around 155 rows with the same situations, which the trip duration is more than 1 day. 
 To make the analysis easier for this case, I decided to removed all the trips that are more than 1 day. The trips could be a result of a system error and such.
 
 ## Data Analyzing
 
Now, the data is cleaned and ready to be analyzed more thoroughly. I will continue using **MSSQL** to break down the data into few tables. This will make it easier for me to do the visualisation later on.
 
1) I started the analysis by calculating how many users are currently using the apps.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/02416869-9e37-4522-a770-90e1d5c3dafd)

2) Then, I did the same calculation to find the number of trips for each months to see the trend in 2022.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/087480d1-6ed0-46bf-9d43-93f5af844504)

3) Next I calculate the number of trips for each days in a week

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/10402b0b-4403-4fb2-b2ff-49362a6a6543)

4) Then I calculate the average trip duration monthly and daily to get a better understanding

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/e7d50eda-5dd3-479c-aa43-98a638adb3d5)

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/408133c4-4e03-46b9-8ee1-be7abb8dfa2e)

5) Calculating the starting hour by users

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/9115c0d8-a745-4ce5-bbb4-8645e90f619f)
 
 6) To find the famous stations or hotspots 
 
 ![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/85008319-20d3-411e-bc85-e699745153c4)


![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/3ee9dce1-ee4b-4b8e-b124-cdf1b83e84f7)


7) Types of bikes used

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/8afb5ee0-bb0c-48a7-9d29-c62bbb49a061)


## Data Visualisation

I saved all the results in xls. format to create charts on Excel for visualisation purpose.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/dc88e990-9595-405c-8ac5-f5d798c12fe5)


Starting off with the main information, we have a total of 4,369,136 users, the members dominating with 59.76% (2,611,119) and 40.24% (1,758,017) of casual users. 
Now let's dive in deeper to see at which months do we get high trips.


![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/6963569c-ae28-4d9a-a3f6-578d6f458e6a)


![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/71520635-5010-40b4-9a24-fa100cbda5e2)

The users are increasing from April up till July then decreases afterwards but still relatively higher than other months. Those months are in the Spring and Summer seasons which make sense 
for the high numbers as the weather is suitable for bike riding. Also, the peak months (June, July) are probably because of the school summer break, where more families or childrens are out
to explore the urban destinations in a leisurely way.

It's also can be seen that the casual users are prone to using the bikes on weekend, while annual members are mostly using the bike on weekdays. We could assume that the annual members are
using the bike as their transportation to the workplace or school, while the casual members are using it for sightseeing purpose.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/79890f9d-1ee9-4948-8bfe-5825f054427e)

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/b89ad155-fabb-42c7-922d-05465fdfa919)

Based on the charts above, it is not surprising that annual members are spending less time on bike, with an average <15 mins daily, as I assumed that they are using it to commute to their workplace, 
which is accessible by bikes.
While casual members are spending almost 2x more than annual for leisure purpose.

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/ad6672ce-990e-4cd6-9c62-a2c04edbfc7f)

Plus if we look at the peak hour based on the chart above, we can see that the number surges at early in the morning (7-8am) which the start of office hour. And the next peak hour is between 4-6pm where people are commuting back home.

Now let's see what type of bikes do the user prefer. Based on the chart below, 

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/0ee39120-a103-4614-8669-29e6b74eefb9)

![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/9a567249-3293-44ea-9aa8-b4c4bc2da27f)

 
![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/eacacd51-27c8-4443-8788-4c1fdf55f470)


![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/5b3a75b0-a344-4fdf-8c00-98db4b9b1678)



