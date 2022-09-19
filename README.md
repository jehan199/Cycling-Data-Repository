# Cycling Data Project (Google Data Analytics Certificate Capstone)

A fictious bike rental company based in Chicago has tasked me with analyizing ride data from the last calendar year. Analysts in the finance department have determined that riders that purchase a yearly membership are more profitable for the company than casual riders who purchase either a one time ride or a day pass. The task at hand is to determine how casual riders differ from members in terms of their riding behavior. The next step after that is to determine how these trends can inform strategies that aim to convert casual riders into yearly members.

## Download the data and upload it into BigQuery

Data on the ride history was provided in a document contained within the certification course. After downloading it to my local machine, I unzipped the files and uploaded them into Google Storage and then into tables in BigQuery.

## Union data into a single table for querying

To work with the entire dataset as one table, I unioned each of the files togther and saved a table with the output.

CREATE TABLE Cycling_Data.swag AS Full_Year_Table

(SELECT *

FROM `cycling-case-study-362219.Cycling_Data.April_2022`

UNION ALL 

SELECT *

FROM `cycling-case-study-362219.Cycling_Data.August_2022`

And so on until all filles had been unioned into one file).

## Swag



---CREATE TABLE Cycling_Data.perm_tbl_for_transpose AS 
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
