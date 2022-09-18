# Cycling Data Project (Google Data Analytics Certificate Capstone)

A fictious bike rental company based in Chicago has tasked me with analyizing ride data from the last calendar year. Analysts in the finance department have determined that riders that purchase a yearly membership are more profitable for the company than casual riders who purchase either a one time ride or a day pass. The task at hand is to determine how casual riders differ from members in terms of their riding behavior. The next step after that is to determine how these trends can inform strategies that aim to convert casual riders into yearly members.

## Download the data and upload it into BigQuery.

Data on the ride history was provided in a document contained within the certification course. After downloading it to my local machine, I unzipped the files and uploaded them into Google Storage and then into tables in BigQuery.

## Union data into a single table for querying.

To work with the entire dataset as one table, I unioned each of the files togther. 

  SELECT *
  
  FROM `cycling-case-study-362219.Cycling_Data.April_2022`
  
  UNION ALL
  
  SELECT *
  FROM `cycling-case-study-362219.Cycling_Data.August_2022`
  UNION ALL
  SELECT *
  FROM `cycling-case-study-362219.Cycling_Data.Dec_2021`
  UNION ALL
  SELECT *
  FROM `cycling-case-study-362219.Cycling_Data.Feb_2022`
  UNION ALL
  SELECT *
  FROM `cycling-case-study-362219.Cycling_Data.Jan_2022`
  UNION ALL 
  SELECT *
  FROM `cycling-case-study-362219.Cycling_Data.July_2022`
  UNION ALL 
  SELECT *
  FROM `cycling-case-study-362219.Cycling_Data.June_2022`
  UNION ALL
  SELECT *
  FROM `cycling-case-study-362219.Cycling_Data.March_2022`
  UNION ALL
  SELECT *
  FROM `cycling-case-study-362219.Cycling_Data.May_2022`
  UNION ALL
  SELECT *
  FROM `cycling-case-study-362219.Cycling_Data.Nov_2021`
  UNION ALL
  SELECT *
  FROM `cycling-case-study-362219.Cycling_Data.Oct_2021`
  UNION ALL
  SELECT *
  FROM `cycling-case-study-362219.Cycling_Data.Sep_2021`
