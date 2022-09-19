# Cycling Data Project (Google Data Analytics Certificate Capstone)

A fictious bike rental company based in Chicago has tasked me with analyizing ride data from the last calendar year. Analysts in the finance department have determined that riders that purchase a yearly membership are more profitable for the company than casual riders who purchase either a one time ride or a day pass. The task at hand is to determine how casual riders differ from members in terms of their riding behavior. The next step after that is to determine how these trends can inform strategies that aim to convert casual riders into yearly members.

## Download the data and upload it into BigQuery

Data on the ride history was provided in a document contained within the certification course. After downloading it to my local machine, I unzipped the files and uploaded them into Google Storage and then into tables in BigQuery.

## Union data into a single table for querying

To work with the entire dataset as one table, I unioned each of the files togther and saved a table with the output.

CREATE TABLE Cycling_Data.swag AS Full_Year_Table
```
(SELECT *
FROM `cycling-case-study-362219.Cycling_Data.April_2022`
UNION ALL
SELECT *
FROM `cycling-case-study-362219.Cycling_Data.August_2022`
And so on until all filles had been unioned into one file).
```
# Write SQL scripts to format data for further analysis in R

## Formating data for assessing the link between riding paterns and temperature 

For this query I sourced data from NOAA's database of historical weather. I grouped the cycling data by date and then pulled in the daily high/low and then averaged the temperature for each date. 

```
CREATE TABLE Cycling_Data.perm_tbl_for_transpose AS 
(WITH date_breakdown AS 
(SELECT 
  EXTRACT(DATE FROM started_at)AS full_date,
  ride_id,
  member_casual
FROM `cycling-case-study-362219.Cycling_Data.Full_Year_Data`)
SELECT
  member_casual, COUNT(ride_id) AS count_of_rides, full_date, AVG((TMAX__Degrees_Fahrenheit_+TMIN__Degrees_Fahrenheit_)/2) AS Average_Temp
FROM date_breakdown AS full_year
LEFT JOIN`cycling-case-study-362219.Cycling_Data.Weather_Data` AS Weather
ON full_year.full_date = weather.date
GROUP BY full_date, member_casual)
```
I then used a second query to pivot the member_casual column to put my data in a format that would be suitable for ploting in R.

```
WITH near_finish AS(SELECT *
FROM 
(SELECT
  member_casual,
  full_date,
  count_of_rides,
  Average_Temp,
FROM
  `cycling-case-study-362219.Cycling_Data.perm_tbl_for_transpose`)
PIVOT
(SUM(count_of_rides) AS rides
FOR member_casual IN ('casual', 'member')))
SELECT *, (rides_member/(rides_casual+rides_member)) AS members_rides_over_total_rides
FROM near_finish
```

## Analyze the query results in R

The output of the second query results in there being 5 columns: full_date,	Average_Temp,	rides_casual,	rides_member,	members_rides_over_total_rides
From these columns I can then run a cor.test to see if higher temperates are corrilated with a lower porportion of total rides being rides taken by members.

```
cor.test(pivoted_cycle_data$Average_Temp,pivoted_cycle_data$member_rides_fraction, method = "pearson")
```
The output is as follows: 
t = -25.018, df = 363, p-value < 2.2e-16
alternative hypothesis: true correlation is not equal to 0
95 percent confidence interval:
 -0.8304023 -0.7545358
sample estimates:
       cor 
-0.7955669

This seems to suggest that higher temperatures are in fact corrilated to a lower porportion of total rides being rides taken by members.
A quick plot of this also shows the same relationship.
```
ggplot(pivoted_cycle_data,mapping = aes(Average_Temp,member_rides_fraction)) +
  geom_point()+
  geom_smooth()+
  labs(title = 'Temperature Impact on Riding Type',x = 'Average Temperature (F)',y = 'Member Rides/Total Rides') +
  theme(plot.title = element_text(hjust = 0.5))  
```
## Takeaway + link to R markdown HTML for visualization

While corrilation does not neccesarilly mean causation, it is indisputably true the higher during the warmer months the percentage of total rides being taken by members decreases. Considering that it is the stated goal of this company to convert casual riders into members, they should consider doing something during the summer months or in the late spring to incentivize their customers to sign up for a membership. Perhaps a discount on the membership or an offer where the first month is free so long as they sign up for a year. 

file:///Users/jeremyhandy/Temp-Impact-on-Riding-Type.html
  
