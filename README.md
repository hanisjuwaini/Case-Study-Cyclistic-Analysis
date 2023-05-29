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
5. I did a run through of the data after the steps above, where I checked for the average of the trip duration.
![image](https://github.com/hanisjuwaini/Case-Study-Cyclistic-Bike-Share-Analysis/assets/87611715/3faccf3a-eb49-4887-96aa-8f3308aa6cf3)
 Then I noticed that the average is 17 minutes, despite the maximum minutes being 34,355 hours which is approximately 23 days. After further checking, there were   
 around 155 rows with the same situations, which the trip duration is more than 1 day. 
 To make the analysis more thourugh, I decided to removed all the trips that are more than 1 day.
 
 ## Data Analyzing
 
 For data analyzing, I used Microsoft SQL for easier process since the dataset is very large,






