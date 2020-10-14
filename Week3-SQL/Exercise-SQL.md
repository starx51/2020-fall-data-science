
# SQL:  Structured Query Language  Exercise

### Getting Started
1. Go to BigQuery UI https://console.cloud.google.com/bigquery
2. Add in the public data sets. 
	3. Click the Add Data icon
	4. Add any dataset
	5. `bigquery-public-data` should become visible and populate in the BigQuery UI. 
3. Add your queries where it says [YOUR QUERY HERE].
4. Make sure you add your query in between the triple tick marks. 
---

For this section of the exercise we will be using the `bigquery-public-data.austin_311.311_service_requests`  table. 

5. Write a query that tells us how many rows are in the table. 
	```
	[
    SELECT *
FROM bigquery-public-data.austin_311.311_service_requests 

  ```  or just display the total count ```
    
SELECT  COUNT(*)  

FROM bigquery-public-data.austin_311.311_service_requests 
]
	```

7. Write a query that tells us how many _distinct_ values there are in the complaint_description column.
	``` 
	[
    SELECT distinct complaint_description
    
FROM bigquery-public-data.austin_311.311_service_requests 

```or we can also order the result alphabetically```

SELECT distinct complaint_description

FROM bigquery-public-data.austin_311.311_service_requests 

order by complaint_description;
]
	```
  
8. Write a query that counts how many times each owning_department appears in the table and orders them from highest to lowest. 
	``` 
	[
SELECT owning_department, count(*)  as Highest_to_lowest

FROM bigquery-public-data.austin_311.311_service_requests 

group by owning_department

order by count(*) desc
]
	```

9. Write a query that lists the top 5 complaint_description that appear most and the amount of times they appear in this table. (hint... limit)
	```
	[
SELECT complaint_description, count(*)  as Highest_to_lowest

FROM bigquery-public-data.austin_311.311_service_requests 

group by complaint_description

order by count(*) desc
limit 5
	  ```
10. Write a query that lists and counts all the complaint_description, just for the where the owning_department is 'Animal Services Office'.
	```
	[SELECT complaint_description, count(*)  as Animals_complaints

FROM bigquery-public-data.austin_311.311_service_requests 

where owning_department = "Animal Services Office"

group by   complaint_description
order by count(*) desc]
	```

11. Write a query to check if there are any duplicate values in the unique_key column (hint.. There are two was to do this, one is to use a temporary table for the groupby, then filter for values that have more than one count, or, using just one table but including the  `having` function). 
	```
	[SELECT unique_key, count(*)  as Duplicates

FROM bigquery-public-data.austin_311.311_service_requests 

group by unique_key
HAVING COUNT(*) > 1
]
	```


### For the next question, use the `census_bureau_usa` tables.

1. Write a query that returns each zipcode and their population for 2000 and 2010. 
	```
	[

select  zipcode, population 
from `bigquery-public-data.census_bureau_usa.population_by_zip_2000` as Zip_pop_2000
UNION ALL
select zipcode, population
from `bigquery-public-data.census_bureau_usa.population_by_zip_2010`  as Zip_pop_2010

]
	```

### For the next section, use the  `bigquery-public-data.google_political_ads.advertiser_weekly_spend` table.
1. Using the `advertiser_weekly_spend` table, write a query that finds the advertiser_name that spent the most in usd. 
	```
	[select advertiser_name, sum(spend_usd) as usd
from `bigquery-public-data.google_political_ads.advertiser_weekly_spend`  
group by advertiser_name
order by sum(spend_usd) desc]
	```
2. Who was the 6th highest spender? (No need to insert query here, just type in the answer.)
	```
	[
    TOM STEYER 2020-------> 8801900 usd
]
	```

3. What week_start_date had the highest spend? (```2020-09-20------->26595500 usd```)
	```
	[
select week_start_date, sum(spend_usd) as usd
from `bigquery-public-data.google_political_ads.advertiser_weekly_spend`  
group by week_start_date
order by sum(spend_usd) desc
]
	```

4. Using the `advertiser_weekly_spend` table, write a query that returns the sum of spend by week (using week_start_date) in usd for the month of August only. 
	```
	[
select week_start_date, sum(spend_usd) as usd

from `bigquery-public-data.google_political_ads.advertiser_weekly_spend`  
where week_start_date BETWEEN '2020-08-01' AND '2020-08-31'
group by week_start_date
order by sum(spend_usd) desc
]
	```
6.  How many ads did the 'TOM STEYER 2020' campaign run? (No need to insert query here, just type in the answer.)
	```
	[50]
    
    select advertiser_name, count(*) as counts
from `bigquery-public-data.google_political_ads.advertiser_weekly_spend`  
where advertiser_name = 'TOM STEYER 2020'
group by advertiser_name
order by count(*) desc
	```
7. Write a query that has, in the US region only, the total spend in usd for each advertiser_name and how many ads they ran. (Hint, you're going to have to join tables for this one). 
	```
		[YOUR QUERY HERE]
	```
8. For each advertiser_name, find the average spend per ad. 
	```
	[ 
select advertiser_name, avg(spend_usd) as Average_spent
from `bigquery-public-data.google_political_ads.advertiser_weekly_spend`  
group by advertiser_name
order by avg(spend_usd) desc]
	```
10. Which advertiser_name had the lowest average spend per ad that was at least above 0. 
	``` 
	[select advertiser_name, min(spend_usd) 
from `bigquery-public-data.google_political_ads.advertiser_weekly_spend`  
group by advertiser_name
order by min(spend_usd) desc]
	```
## For this next section, use the `new_york_citibike` datasets.

1. Who went on more bike trips, Males or Females?
	```
	[Select gender, count(*) as n_gender
from `bigquery-public-data.new_york_citibike.citibike_trips` 
group by gender;

]
	```
2. What was the average, shortest, and longest bike trip taken in minutes?
	```
	[select count(*) as count, 
round(avg(tripduration),2) as Avg , 
min(tripduration) as min,
max(tripduration) as max
from `bigquery-public-data.new_york_citibike.citibike_trips` 
]
	```

3. Write a query that, for every station_name, has the amount of trips that started there and the amount of trips that ended there. (Hint, use two temporary tables, one that counts the amount of starts, the other that counts the number of ends, and then join the two.) 
	```
	with TABLE_S as(
select start_station_name,
count(*) as start_sation
from `bigquery-public-data.new_york_citibike.citibike_trips`
group by start_statio_name
TaBLE_E as ( select end_station_name,
count(*) as end_station_name, 
from `bigquery-public-data.new_york_citibike.citibike_trips` 
group by end_station_name
)
select A.start_station_name, A.start_station, B.end_station_name from TABLE_S as A join TABLE_E as B on A.start_station_name = B.end_station_name



]
	```
# The next section is the Google Colab section.  
1. Open up this [this Colab notebook](https://colab.research.google.com/drive/1kHdTtuHTPEaMH32GotVum41YVdeyzQ74?usp=sharing).
2. Save a copy of it in your drive. 
3. Rename your saved version with your initials. 
4. Click the 'Share' button on the top right.  
5. Change the permissions so anyone with link can view. 
6. Copy the link and paste it right below this line. 
	* YOUR LINK:  ________________________________
9. Complete the two questions in the colab notebook file. 
