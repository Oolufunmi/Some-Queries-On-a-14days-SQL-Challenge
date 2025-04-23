# Some-Queries-On-a-14days-SQL-Challenge
Completed the 14-Day SQL Challenge with Silvia on linkedin (14daysSQlwithsilvia), where I tackled real-world SQL problems daily using SELECT, JOIN, GROUP BY, and subqueries. This hands-on challenge sharpened my skills in querying, data analysis, and logical thinking using structured, practical datasets.
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
### Explanation of Date Parts in SQLlite
1. %Y → Year (e.g., 2024)
2. %m → Month (01-12)
3. %d → Day of the month (01-31)
4. %H → Hour (00-23)
5. %M → Minute (00-59)
6. %S → Second (00-59)
7. %w → Weekday (0 = Sunday, 6 = Saturday)
8. %j → Day of the year (001-366)
9. %W → Week of the year (00-53)

### Things i learnt
Day 8 - i learnt that group by is used to summarize (like pivottable), orderby is used to arrange
Day 9 - Case when is used when you need to filter with a row
(COUNT(CASE WHEN  is used when there are multiple options in a column. And you need to filter base on this options


## DAy 1 SQL CHALLENGE Day 1 (
To execute coding effective:
 always interpret the questions 
Understand the coding logic to use
 break it down into stages
 Execute
 
### learning s from this challenge
I learnt that there are 2 major types of error in coding
Syntax error
Logical error
The fact that you could successfully run a code doesn't imply that the answer is correct
And also learnt that
Understanding the question is one thing, writing the queries is another thing, answering the question with the right query is another thing.
I think i prefer reading how to code query to watching youtube videos or watching videos

### SQL CHALLENGE Day 2
Question 1
What is the minimum host response time in hours for guest inquiries in January 2024? This metric will help identify if any hosts are setting an exceptionally quick response standard.

```mysql
SELECT min(response_time_hours) FROM `fct_guest_inquiries (Day2)`WHERE inquiry_date BETWEEN '2024-01-01' AND '2024-01-31';
```
Question 2
For guest inquiries made in January 2024, what is the average host response time rounded to the nearest hour? This average will provide insight into overall host responsiveness.
```mysql
SELECT round(AVG(response_time_hours)) FROM `fct_guest_inquiries (Day2)`WHERE inquiry_date BETWEEN '01-01-2024' AND '31-01-2024';
```
Question 3
List the inquiry_id and response_time_hours for guest inquiries made between January 16th and January 31st, 2024 that took longer than 2 hours to respond. This breakdown will help pinpoint hosts with slower response times.
``` mysql
SELECT inquiry_id,response_time_hours FROM `fct_guest_inquiries (Day2)` WHERE inquiry_date BETWEEN '16-01-2024' and '31-01-2024' AND response_time_hours>2;
```

2nd code(this gives amore accurate result because my sql date format is YYYY-MM-DDand the data is in a different format entirely. I was able to use th

```mysql
SELECT inquiry_id, response_time_hours 
FROM `fct_guest_inquiries (Day2)` 
WHERE STR_TO_DATE(inquiry_date, '%d/%m/%Y') 
      BETWEEN STR_TO_DATE('16/01/2024', '%d/%m/%Y') 
          AND STR_TO_DATE('31/01/2024', '%d/%m/%Y') 
AND response_time_hours > 2;
```

### SQL CHALLENGE Day 3
Question 1
What is the aggregated view events across each content category in August 2024? This information will help the Prime Video team understand which content genres are engaging users during that month.

```mysql
SELECT category, sum(views) as Totalviews FROM `content_views_daily_agg(Day 3)` WHERE STR_TO_DATE(view_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/08/2024', '%d/%m/%Y') AND STR_TO_DATE('31/08/2024', '%d/%m/%Y') GROUP by category;
```

Question 2
Which content categories accumulated over 100,000 total views during the third quarter of 2024? This analysis will help identify the genres that are attracting a high volume of viewer engagement.

```mysql
SELECT category, sum(views) FROM `content_views_daily_agg(Day 3)` WHERE STR_TO_DATE(view_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/07/2024', '%d/%m/%Y') AND STR_TO_DATE('30/09/2024', '%d/%m/%Y') GROUP by category HAVING sum(views) > 100000;
```
Question 3

In September 2024, for content categories that received more than 500,000 aggregated views, what is the total views for the month and the average number of views per day?
```mysql
SELECT category,sum(views), avg(views)FROM `content_views_daily_agg(Day 3)` WHERE STR_TO_DATE(view_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/09/2024', '%d/%m/%Y') AND STR_TO_DATE('30/09/2024', '%d/%m/%Y') GROUP by category HAVING sum(views) > 500000;
```


### SQL CHALLENGE Day 4
Question 1
How many unique artists were recommended to users in April 2024? This analysis will help determine the diversity of recommendations during that month.
```mysql
SELECT count(DISTINCT(artist_id)) FROM `fct_artist_recommendations(Day4)` where STR_TO_DATE(recommendation_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/04/2024', '%d/%m/%Y') AND STR_TO_DATE('30/04/2024', '%d/%m/%Y');
```
Question 2
2️⃣Question - What is the total number of recommendations for new artists in May 2024?
```mysql
SELECT count(recommendation_id) as TotalNos FROM `fct_artist_recommendations(Day4)` where STR_TO_DATE(recommendation_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/05/2024', '%d/%m/%Y') AND STR_TO_DATE('31/05/2024', '%d/%m/%Y') GROUP by is_new_artist HAVING is_new_artist = '1';
```
There is no point using Group by and Having in this query
So this is the correct code 

```mysql
SELECT COUNT(recommendation_id) AS TotalNos 
FROM `fct_artist_recommendations(Day4)` 
WHERE STR_TO_DATE(recommendation_date, '%d/%m/%Y') 
      BETWEEN STR_TO_DATE('01/05/2024', '%d/%m/%Y') 
          AND STR_TO_DATE('31/05/2024', '%d/%m/%Y') 
      AND is_new_artist = '1';
```
QUESTION 3
3️⃣Question - For each month in Quarter 2 2024 (April through June 2024), how many distinct new artists were recommended to users?

To answer this question , i couldn't attempt it directly
I have to write the code to filter by month only using question 2 as template, since question 3 is a continuation of question 2, only that question 3 want the recommendations of all users in the quarter and not just May 2024
So i wrote the code for just may first
Then i had to use date format to convert my date to month and year alone instead of the initial day, month and time.
```mysql
SELECT COUNT(recommendation_id) AS TotalNos, DATE_FORMAT(STR_TO_DATE(recommendation_date, '%d/%m/%Y'), '%Y-%m') as Month FROM `fct_artist_recommendations(Day4)` WHERE DATE_FORMAT(STR_TO_DATE(recommendation_date, '%d/%m/%Y'), '%Y-%m') = '2024-05' AND is_new_artist = '1' GROUP by Month;
```


Question 3
For each month in Quarter 2 2024 (April through June 2024), how many distinct new artists were recommended to users?
```mysql
SELECT DATE_FORMAT(STR_TO_DATE(recommendation_date, '%d/%m/%Y'),'%Y-%m') As EachMonth, COUNT(recommendation_id) AS TotalNos FROM `fct_artist_recommendations(Day4)` WHERE DATE_FORMAT(STR_TO_DATE(recommendation_date, '%d/%m/%Y'), '%Y-%m') BETWEEN '2024-04'and '2024-06' AND is_new_artist = '1' group by EachMonth ORDER by EachMonth;
```

### SQL CHALLENGE Day 5
(Its like introduction to subquery)
Group by - Filter, Order by - Sort
Question 1

What are the names of the 3 pharmacies that conducted the fewest number of consultations in July 2024? This will help us identify locations with potentially less crowded consultation spaces.
```mysql
SELECT pharmacy_name, count(pharmacy_name)AS FewNos FROM `fct_consultations(Day5)` where STR_TO_DATE(consultation_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/07/2024', '%d/%m/%Y') AND STR_TO_DATE('31/07/2024', '%d/%m/%Y') GROUP by pharmacy_name ORDER by FewNos ASC limit 3;
```
Question 2
2️⃣Question - For the pharmacies identified in the previous question (i.e., the 3 pharmacies with the fewest consultations in July 2024), what is the uppercase version of the consultation room types available? Understanding the room types can provide insights into the privacy features offered.

THIS ANSWER HAS SUBQUERY IN IT
```mysql
SELECT DISTINCT UPPER(c.consultation_room_type) AS room_type 
FROM `fct_consultations(Day5)` c JOIN 
( SELECT pharmacy_name FROM `fct_consultations(Day5)` WHERE STR_TO_DATE(consultation_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/07/2024', '%d/%m/%Y') 
AND STR_TO_DATE('31/07/2024', '%d/%m/%Y') 
GROUP BY pharmacy_name ORDER BY COUNT(pharmacy_name) ASC LIMIT 3 ) AS subquery ON c.
pharmacy_name = subquery.pharmacy_name;
```
NB; c. means alias and it is used when you have subqueries

Question 3
So far, we have identified the 3 pharmacies with a fewest consultations in July 2024. Among these 3 pharmacies, what is the minimum privacy level score for each consultation room type in July 2024?
```mysql
SELECT c.consultation_room_type, min(c.privacy_level_score) AS LowlevelScore FROM `fct_consultations(Day5)` c JOIN ( SELECT pharmacy_name FROM `fct_consultations(Day5)` WHERE STR_TO_DATE(consultation_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/07/2024', '%d/%m/%Y') AND STR_TO_DATE('31/07/2024', '%d/%m/%Y') GROUP BY pharmacy_name ORDER BY COUNT(pharmacy_name) ASC LIMIT 3 ) AS subquery ON c. pharmacy_name = subquery.pharmacy_name GROUP by c.consultation_room_type;
```


### SQL CHALLENGE Day 6
Question 1
We want to evaluate early premium service adoption among enterprise customers. How many unique customers with service tier codes starting with ''PREM'' completed transactions from April 1st to April 30th, 2024?
```mysql
SELECT count(DISTINCT(customer_id)) FROM `fct_transactions(Day6)` WHERE STR_TO_DATE(transaction_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/04/2024', '%d/%m/%Y') AND STR_TO_DATE('30/04/2024', '%d/%m/%Y') and service_tier_code LIKE'PREM%';
```
Question 2
We need to understand usage trends for different service tiers in order to refine our service packages. Which service tier codes were used most frequently by customers for transactions in May 2024, ranked from most to least frequent?

```mysql
SELECT service_tier_code,count(service_tier_code) as UseageTrend FROM `fct_transactions(Day6)` WHERE STR_TO_DATE(transaction_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/05/2024', '%d/%m/%Y') AND STR_TO_DATE('31/05/2024', '%d/%m/%Y') GROUP by service_tier_code order by UseageTrend DESC;
```
Question 3
We want to pinpoint the most active service tiers to inform pricing adjustments for enterprise cloud offerings. For transactions between June 1st and June 30th, 2024, what are the top three service tier codes based on transaction volume and how many transactions were recorded for each?
```mysql
SELECT service_tier_code,COUNT(transaction_id) AS TransactionVolume FROM `fct_transactions(Day6)` WHERE STR_TO_DATE(transaction_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/06/2024', '%d/%m/%Y') AND STR_TO_DATE('30/06/2024', '%d/%m/%Y') GROUP by service_tier_code order by TransactionVolume DESC LIMIT 3;
```
### SQL CHALLENGE Day 7
Question 1
In October 2024, what is the fastest installation time recorded for a Windows update? This metric will help us benchmark the best-case update performance for our deployment strategy.
```mysql
SELECT MIN(installation_time_minutes) AS FastestTime 
FROM fct_update_installations 
WHERE installation_date BETWEEN '2024-10-01' AND '2024-10-31';
```
Question 2
In November 2024, how many unique users successfully installed a Windows update without encountering any installation errors? This figure will help us assess update reliability.
```mysql
SELECT count(DISTINCT(user_id)) FROM`fct_update_installations(Day 7)` WHERE STR_TO_DATE(installation_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/11/2024', '%d/%m/%Y') AND STR_TO_DATE('30/11/2024', '%d/%m/%Y') and installation_error = '0';
```
Question 3
In December 2024, what is the fastest installation time for Windows updates among users who did not experience any errors? This metric will directly inform our update communication and deployment strategy to increase adoption rates.
```mysql
SELECT min(installation_time_minutes) as FastestTimetaken FROM `fct_update_installations(Day 7)` WHERE STR_TO_DATE(installation_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/12/2024', '%d/%m/%Y') AND STR_TO_DATE('31/12/2024', '%d/%m/%Y') and installation_error = '0';
```

### SQL CHALLENGE Day 8(i used Join)- i need to retry this day 8 again without chatgpt
Question 1
What is the average length of the file names shared across different organizational segments in January 2024?
```mysql
SELECT `dim_organization(Day8)`.segment,Avg(length(file_name)) as Namelength FROM `fct_file_sharing(Day8)` JOIN `dim_organization(Day8)` ON `fct_file_sharing(Day8)`.organization_id = `dim_organization(Day8)`.organization_id WHERE STR_TO_DATE(shared_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/01/2024', '%d/%m/%Y') AND STR_TO_DATE('31/01/2024', '%d/%m/%Y') GROUP by `dim_organization(Day8)`.segment;
```
Question 2(i used Concatenation)
How many files were shared with names that start with the same prefix as the organization name, concatenated with a hyphen, in February 2024?
```mysql
SELECT count(DISTINCT(file_name)) FROM `fct_file_sharing(Day8)` JOIN `dim_organization(Day8)` ON `fct_file_sharing(Day8)`.organization_id =`dim_organization(Day8)`.organization_id WHERE STR_TO_DATE(shared_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/02/2024', '%d/%m/%Y') AND STR_TO_DATE('29/02/2024', '%d/%m/%Y') AND file_name LIKE CONCAT(`dim_organization(Day8)`.organization_name, '-%');
```
Question 3
Identify the top 3 organizational segments with the highest number of files shared where the co-editing user is NULL, indicating a potential security risk, during the first quarter of 2024.
```mysql
SELECT dim_organization.segment, COUNT(fct_file_sharing.file_id) AS file_count FROM fct_file_sharing JOIN dim_organization ON fct_file_sharing.organization_id = dim_organization.organization_id WHERE STR_TO_DATE(fct_file_sharing.shared_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/02/2024', '%d/%m/%Y') AND STR_TO_DATE('29/02/2024', '%d/%m/%Y') GROUP BY dim_organization.segment ORDER BY file_count DESC LIMIT 3;
```
```mysql
SELECT dim_organization.segment, COUNT(fct_file_sharing.file_id) AS file_count FROM fct_file_sharing JOIN dim_organization ON fct_file_sharing.organization_id = dim_organization.organization_id WHERE STR_TO_DATE(fct_file_sharing.shared_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/01/2024', '%d/%m/%Y') AND STR_TO_DATE('31/03/2024', '%d/%m/%Y') AND fct_file_sharing.co_editing_user_id IS NULL GROUP BY dim_organization.segment ORDER BY file_count DESC LIMIT 3;
```
### SQL CHALLENGE Day 8 (Reattempting SQL Challenge day 8 without Chagqpt)
Question 1(using sqllite)
Average in statistics means total / count of observation
```mysql
select dim_organization.segment, AVG(LENGTH(file_name)) AS Average from fct_file_sharing JOIN dim_organization ON dim_organization.organization_id = fct_file_sharing.organization_id WHERE STR_TO_DATE(fct_file_sharing.shared_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/01/2024', '%d/%m/%Y') AND STR_TO_DATE('31/01/2024', '%d/%m/%Y') GROUP BY dim_organization.segment;
```
Question 2

### SQL CHALLENGE Day 9
NB: group by is used to summarize (like pivottable), orderby is used to arrange
Question 1
For Quarter 3 of 2024, what is the total number of distinct customers who started a subscription for each pricing tier? This query establishes baseline subscription counts for evaluating customer retention.
```mysql
SELECT pricing_tier, COUNT(DISTINCT customer_id) AS total_customers FROM `fct_subscriptions(Day9)` WHERE STR_TO_DATE(start_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/07/2024', '%d/%m/%Y') AND STR_TO_DATE('30/09/2024', '%d/%m/%Y') GROUP BY pricing_tier;
```
Question 2(this question filters and calculate %)
For each pricing tier in Quarter 3 of 2024, what percentage of customers renewed their subscription? Customers who have renewed their subscription would have a renewal status of 'Renewed'. This breakdown will help assess retention effectiveness across tiers.
This is what this question means
so question two means we are counting those who renewed while we are stilll filtering by pricing tiers 
For Question 2, we are:
Filtering by pricing tier → Grouping subscriptions based on their pricing tier.


Counting only those who renewed → Using COUNT(CASE WHEN renewal_status = 'Renewed' THEN customer_id END).


Dividing by total customers in that pricing tier → To get the renewal percentage.


This tells us what percentage of customers in each pricing tier renewed their subscriptions.
I am yet to calculate the % here
```mysql
SELECT renewal_status,count(DISTINCT(customer_id)) FROM `fct_subscriptions(Day9)` WHERE STR_TO_DATE(start_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/07/2024', '%d/%m/%Y') AND STR_TO_DATE('30/09/2024', '%d/%m/%Y') GROUP by renewal_status;
```
The final query
```mysql
SELECT pricing_tier, (COUNT(CASE WHEN renewal_status = 'Renewed' THEN customer_id END) / COUNT(DISTINCT customer_id)) * 100 AS renewal_rate FROM `fct_subscriptions(Day9)` WHERE STR_TO_DATE(start_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/07/2024', '%d/%m/%Y') AND STR_TO_DATE('30/09/2024', '%d/%m/%Y') GROUP BY pricing_tier;
```

Question 3
Based on subscriptions that started in Quarter 3 of 2024, rank the pricing tiers by their retention rate. We’d like to see both the retention rate and the rank for each tier, so we can identify which pricing model keeps customers engaged the longest.
```mysql
SELECT pricing_tier, renewal_rate, RANK() OVER (ORDER BY renewal_rate DESC) AS `rank` FROM ( SELECT pricing_tier, (COUNT(CASE WHEN renewal_status = 'Renewed' THEN customer_id END) / COUNT(DISTINCT customer_id)) * 100 AS renewal_rate FROM `fct_subscriptions(Day9)` WHERE STR_TO_DATE(start_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/07/2024', '%d/%m/%Y') AND STR_TO_DATE('30/09/2024', '%d/%m/%Y') GROUP BY pricing_tier ) AS Renewal_data;
```

Query for mysql
```mysql
SELECT pricing_tier, renewal_rate, RANK() OVER (ORDER BY renewal_rate DESC) AS `rank` FROM ( SELECT pricing_tier, (COUNT(CASE WHEN renewal_status = 'Renewed' THEN customer_id END) / COUNT(DISTINCT customer_id)) * 100 AS renewal_rate FROM `fct_subscriptions(Day9)` WHERE STR_TO_DATE(start_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/07/2024', '%d/%m/%Y') AND STR_TO_DATE('30/09/2024', '%d/%m/%Y') GROUP BY pricing_tier ) AS Renewal_data;
```

The query for sqllite
```mysql
SELECT 
    pricing_tier, 
    renewal_rate, 
    RANK() OVER (ORDER BY renewal_rate DESC) AS "rank"
FROM (
    SELECT 
        pricing_tier, 
        (COUNT(CASE WHEN renewal_status = 'Renewed' THEN customer_id END) 
        * 1.0 / COUNT(DISTINCT customer_id)) * 100 AS renewal_rate 
    FROM fct_subscriptions  
    WHERE start_date BETWEEN '2024-07-01' AND '2024-09-30'  
    GROUP BY pricing_tier  
) AS Renewal_data;
Using Subquery to answer the question
SELECT pricing_tier,RetentionRate,  RANK() OVER (ORDER BY RetentionRate DESC) AS RankValue 
  FROM ( 
  SELECT pricing_tier,
    (COUNT(CASE WHEN renewal_status = 'Renewed' THEN customer_id END) / COUNT(customer_id)) * 100 AS RetentionRate
FROM 
    fct_subscriptions 
WHERE 
    start_date BETWEEN '2024-07-01' AND '2024-09-30'
GROUP BY 
    pricing_tier) AS Subquery ;
 ```

### SQL CHALLENGE Day 10
Question 1
Retrieve the total marketing spend in each country for Q1 2024 to help inform budget distribution across regions.
```mysql
SELECT country_name, sum(amount_spent) AS TotalAmountSpent FROM `fact_marketing_spend(Day10)` JOIN `dimension_country(Day10)` ON `dimension_country(Day10)`.country_id = `fact_marketing_spend(Day10)`.country_id WHERE STR_TO_DATE(campaign_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/01/2024', '%d/%m/%Y') AND STR_TO_DATE('31/03/2024', '%d/%m/%Y') GROUP BY country_name;
```
Question 2
List the number of new subscribers acquired in each country (with name) during January 2024, renaming the subscriber count column to 'new_subscribers' for clearer reporting purposes.
```mysql
SELECT country_name,COUNT(num_new_subscribers) FROM `fact_daily_subscriptions(Day10)`join `dimension_country(Day10)` on `dimension_country(Day10)`.country_id = `fact_daily_subscriptions(Day10)`.country_id WHERE STR_TO_DATE(signup_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/01/2024', '%d/%m/%Y') AND STR_TO_DATE('31/01/2024', '%d/%m/%Y') GROUP BY country_name;
```
Question 3
Determine the average marketing spend per new subscriber for each country in Q1 2024 by rounding up to the nearest whole number to evaluate campaign efficiency.
I can use round or ceil to round up to the nearest whole number in mysql
USING CEIL
```mysql
SELECT country_name, CEIL(sum(amount_spent)/ NULLIF(sum(num_new_subscribers), 0))AS SubsAmountPerUser FROM `fact_marketing_spend(Day10)` join `dimension_country(Day10)` on `dimension_country(Day10)`.country_id = `fact_marketing_spend(Day10)`.country_id join `fact_daily_subscriptions(Day10)` on `fact_daily_subscriptions(Day10)`.country_id = `fact_marketing_spend(Day10)`.country_id WHERE STR_TO_DATE(campaign_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/01/2024', '%d/%m/%Y') AND STR_TO_DATE('31/03/2024', '%d/%m/%Y') and STR_TO_DATE(signup_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/01/2024', '%d/%m/%Y') AND STR_TO_DATE('31/03/2024', '%d/%m/%Y') GROUP by country_name;
```


USING ROUND
```mysql
SELECT country_name, round(sum(amount_spent)/ NULLIF(sum(num_new_subscribers), 0))AS SubsAmountPerUser FROM `fact_marketing_spend(Day10)` join `dimension_country(Day10)` on `dimension_country(Day10)`.country_id = `fact_marketing_spend(Day10)`.country_id join `fact_daily_subscriptions(Day10)` on `fact_daily_subscriptions(Day10)`.country_id = `fact_marketing_spend(Day10)`.country_id WHERE STR_TO_DATE(campaign_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/01/2024', '%d/%m/%Y') AND STR_TO_DATE('31/03/2024', '%d/%m/%Y') and STR_TO_DATE(signup_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/01/2024', '%d/%m/%Y') AND STR_TO_DATE('31/03/2024', '%d/%m/%Y') GROUP by country_name;
```




### SQL CHALLENGE Day 11
Question 1
We need to know who our most active suppliers are. Identify the top 5 suppliers based on the total volume of components delivered in October 2024.
```mysql
SELECT Supplier_name, sum(component_count) AS ActiveSuppliers FROM `supplier_deliveries(Day11)` JOIN `suppliers(Day11)` on `suppliers(Day11)`.supplier_id = `supplier_deliveries(Day11)`.supplier_id WHERE STR_TO_DATE(delivery_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/10/2024', '%d/%m/%Y') AND STR_TO_DATE('31/10/2024', '%d/%m/%Y') GROUP BY supplier_name ORDER BY ActiveSuppliers DESC LIMIT 5;
```

Question 2(I NEED TO LEARN CTE)
For each region, find the supplier ID that delivered the highest number of components in November 2024. This will help us understand which supplier is handling the most volume per market.
```mysql
WITH RegionalSupplierTotals AS ( SELECT manufacturing_region, -- This should be your region/market column. supplier_id, SUM(component_count) AS total_components, ROW_NUMBER() OVER ( PARTITION BY manufacturing_region ORDER BY SUM(component_count) DESC ) AS rn FROM supplier_deliveries WHERE STR_TO_DATE(delivery_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/11/2024', '%d/%m/%Y') AND STR_TO_DATE('30/11/2024', '%d/%m/%Y') GROUP BY manufacturing_region, supplier_id ) SELECT manufacturing_region, supplier_id, total_components FROM RegionalSupplierTotals WHERE rn = 1;
```
What i wrote by myself (this code showed the ranking)in MySQL
```mysql
with MarketingSales AS ( SELECT supplier_id, manufacturing_region, sum(component_count) As TotalSales FROM `supplier_deliveries` WHERE STR_TO_DATE(delivery_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/11/2024', '%d/%m/%Y') AND STR_TO_DATE('30/11/2024', '%d/%m/%Y') GROUP by supplier_id,manufacturing_region ) Select supplier_id, manufacturing_region,MAX(TotalSales) FROM MarketingSales GROUP BY manufacturing_region;
```

Question 3 (i used “NOT IN” here) AND A SUBQUERY
We need to identify potential gaps in our supply chain for Asia. List all suppliers by name who have not delivered any components to the 'Asia' manufacturing region in December 2024.
```mysql
SELECT supplier_name FROM suppliers WHERE supplier_id NOT IN( SELECT supplier_id FROM `supplier_deliveries` where manufacturing_region = 'Asia' AND STR_TO_DATE(delivery_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/12/2024', '%d/%m/%Y') AND STR_TO_DATE('31/12/2024', '%d/%m/%Y') );
```

STEPS TO ANSWER QUESTION 2  USING CTE
Stage 1: Filter and Aggregate Data
First, we filter the data for November 2024 and calculate the total components delivered by each supplier in each region.
```mysql
SELECT manufacturing_region, supplier_id, sum(component_count) FROM `supplier_deliveries` WHERE STR_TO_DATE(delivery_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/11/2024', '%d/%m/%Y') AND STR_TO_DATE('30/11/2024', '%d/%m/%Y') GROUP BY manufacturing_region,supplier_id;
```
Stage 2: Rank Suppliers Within Each Region
Next, we rank the suppliers within each region based on the total components delivered.
```mysql
SELECT 
    manufacturing_region, 
    supplier_id, 
    total_components,
    ROW_NUMBER() OVER (PARTITION BY manufacturing_region ORDER BY total_components DESC) AS rn
FROM (
    SELECT 
        manufacturing_region, 
        supplier_id, 
        SUM(component_count) AS total_components
    FROM supplier_deliveries
    WHERE delivery_date BETWEEN '2024-11-01' AND '2024-11-30'
    GROUP BY manufacturing_region, supplier_id
) AS AggregatedData;
```
Purpose: This query uses a window function ROW_NUMBER() to assign a rank to each supplier within their respective regions based on the total components delivered, ordered in descending order.
Stage 3: Select Top Supplier per Region
Finally, we select the top supplier in each region by filtering for the rank of 1.
```mysql
WITH RegionalSupplierTotals AS (
    SELECT 
        manufacturing_region, 
        supplier_id, 
        total_components,
        ROW_NUMBER() OVER (PARTITION BY manufacturing_region ORDER BY total_components DESC) AS rn
    FROM (
        SELECT 
            manufacturing_region, 
            supplier_id, 
            SUM(component_count) AS total_components
        FROM supplier_deliveries
        WHERE delivery_date BETWEEN '2024-11-01' AND '2024-11-30'
        GROUP BY manufacturing_region, supplier_id
    ) AS AggregatedData
)
SELECT manufacturing_region, supplier_id, total_components
FROM RegionalSupplierTotals
WHERE rn = 1;
```
Purpose: This final query selects the supplier with the highest total components delivered in each region by filtering for rows where the rank (rn) is 1.
By breaking it down into these stages, you can see how the data is filtered, aggregated, ranked, and finally selected to identify the top supplier in each region. Let me know if you have any more questions!

### SQL CHALLENGE Day 12
Question 1
As a Product Analyst on the Mac software team, you need to understand the engagement of professional content creators with multimedia tools. What is the number of distinct users on the last day in July 2024?
Tables
```mysql
SELECT count(DISTINCT(user_id)) FROM `fct_multimedia_usage` WHERE STR_TO_DATE(usage_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('31/07/2024', '%d/%m/%Y') AND STR_TO_DATE('31/07/2024', '%d/%m/%Y');
```
Question 2
As a Product Analyst on the Mac software team, you are assessing how much time professional content creators spend using multimedia tools. What is the average number of hours spent by users during August 2024? Round the result up to the nearest whole number.
```mysql
SELECT round(avg(hours_spent))) FROM `fct_multimedia_usage` WHERE STR_TO_DATE(usage_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/08/2024', '%d/%m/%Y') AND STR_TO_DATE('31/08/2024', '%d/%m/%Y');
```
```mysql
SELECT round(sum(hours_spent))/ count(user_id)) FROM `fct_multimedia_usage` WHERE STR_TO_DATE(usage_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/08/2024', '%d/%m/%Y') AND STR_TO_DATE('31/08/2024', '%d/%m/%Y');
```
```mysql
SELECT CEIL(AVG(hours_spent)) AS TimeSpent
FROM fct_multimedia_usage
WHERE usage_date BETWEEN '2024-08-01' AND '2024-08-30';
```
Question 3
Question 3 - For each day, determine the distinct user count and the total hours spent using multimedia tools. Which days have both metrics above the respective average daily values for September 2024?


1st part
```mysql
SELECT count(DISTINCT(user_id)) AS UniqueUser, sum(hours_spent) AS TotalCount FROM `fct_multimedia_usage` WHERE STR_TO_DATE(usage_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/09/2024', '%d/%m/%Y') AND STR_TO_DATE('30/09/2024', '%d/%m/%Y') GROUP BY usage_date;
```
 Final Answer for Question 3 
 ```mysql
WITH DailyMetrics AS ( 
SELECT STR_TO_DATE(usage_date, '%d/%m/%Y') 
AS usage_date, COUNT(DISTINCT user_id) AS UniqueUser, 
SUM(hours_spent) AS TotalCount FROM fct_multimedia_usage 
WHERE STR_TO_DATE(usage_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/09/2024', '%d/%m/%Y') AND STR_TO_DATE('30/09/2024', '%d/%m/%Y') GROUP BY STR_TO_DATE(usage_date, '%d/%m/%Y') 
), 
Average AS ( SELECT AVG(UniqueUser) AS AvgUniqueUser, AVG(TotalCount) AS AvgTotalCount FROM DailyMetrics ) 
SELECT DATE_FORMAT(DailyMetrics.usage_date, '%d/%m/%Y') AS usage_date, DailyMetrics.UniqueUser, DailyMetrics.TotalCount FROM DailyMetrics JOIN Average ON DailyMetrics.UniqueUser > Average.AvgUniqueUser AND DailyMetrics.TotalCount > Average.AvgTotalCount ORDER BY DailyMetrics.usage_date;
```

My query after my training with femi for question 3
```mysql
WITH DailyReport AS (
  SELECT 
    usage_date, 
    COUNT(DISTINCT user_id) AS UserCount, 
    SUM(hours_spent) AS TotalHours
  FROM `fct_multimedia_usage`
  WHERE STR_TO_DATE(usage_date, '%d/%m/%Y') 
        BETWEEN STR_TO_DATE('01/09/2024', '%d/%m/%Y') 
        AND STR_TO_DATE('30/09/2024', '%d/%m/%Y')
  GROUP BY usage_date
)
SELECT 
  usage_date, 
  UserCount, 
  TotalHours
FROM DailyReport
HAVING 
  UserCount > (
    SELECT AVG(UserCount) FROM DailyReport
  )
  AND TotalHours > (
    SELECT AVG(TotalHours) FROM DailyReport
  );
```
### SQL CHALLENGE Day 13

Question 1
How many total ad impressions did we receive from custom audience segments in October 2024?
```mysql
SELECT `audience_segments`.segment_name,sum(impressions) FROM `ad_performance` JOIN `audience_segments` ON `audience_segments`.audience_segment_id = `ad_performance`.audience_segment_id where STR_TO_DATE(date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/10/2024', '%d/%m/%Y') AND STR_TO_DATE('31/10/2024', '%d/%m/%Y') AND `audience_segments`.segment_name like '%custom Audience%' GROUP BY `audience_segments`.segment_name;
```
FOR SQLLITE
```mysql
SELECT sum(impressions) 
  FROM ad_performance
  JOIN audience_segments 
  ON audience_segments.audience_segment_id = ad_performance.audience_segment_id 
  where date LIKE '2024-10%'
  AND audience_segments.segment_name 
  LIKE 'Custom Audience%';
```
Question 2
What is the total number of conversions we achieved from each custom audience segment in October 2024?
```mysql
 SELECT audience_segments.segment_name, SUM(conversions) 
   FROM ad_performance 
JOIN audience_segments
   ON audience_segments.audience_segment_id = ad_performance.audience_segment_id 
WHERE date LIKE '2024-10%'
GROUP BY audience_segments.segment_name;
```
Question 3
For each custom audience or lookalike segment, calculate the cost per conversion. Only return this for segments that had non-zero spend and non-zero conversions.
Cost per conversion = Total ad spend / Total number of conversions
```mysql
WITH CostP AS (
  SELECT 
    audience_segments.segment_name,
    SUM(conversions) AS TotalConversion,
    SUM(ad_spend) AS TotalAds 
  FROM `ad_performance` 
  JOIN `audience_segments` 
    ON `audience_segments`.audience_segment_id = `ad_performance`.audience_segment_id 
  WHERE 
    audience_segments.segment_name LIKE 'Custom Audience%' 
    OR audience_segments.segment_name LIKE 'Lookalike Audience%'
  GROUP BY audience_segments.segment_name
)

SELECT 
  segment_name,
  TotalAds / TotalConversion AS CostPerConver 
FROM CostP 
WHERE 
  TotalAds > 0 AND TotalConversion > 0;

```

### SQL CHALLENGE Day 14(CASE WHEN I S USED WHEN I NEED TO FILTER WITHIN A ROW
Question 1
What percentage of user queries in July 2024 were related to either 'technology' or 'science' domains?
```mysql
SELECT 
  (COUNT(CASE WHEN query_domain IN ('technology', 'science') THEN 1 END) * 100.0 / COUNT(*)) AS percentage_queries
FROM fct_queries
WHERE query_timestamp LIKE '2024-07%';
```
Question 2
Calculate the total number of queries per month in Q3 2024. Which month had the highest number of queries?
```mysql
 SELECT COUNT(query_id) AS TotalQuery,
   strftime('%Y-%m', query_timestamp) AS Month 
   FROM fct_queries
   WHERE query_timestamp BETWEEN '2024-07-01' AND '2024-09-30'
GROUP BY Month 
ORDER TotalQuery DESC 
LIMIT 1;
```
Question 3

Find the subscription tier with the highest number of cancellations during Quarter 3 2024 (July 2024 through September 2024). This query will guide retention strategies by identifying the tier with the most significant dropout case.
```mysql
SELECT tier_name,COUNT(tier_name) AS Highesttier FROM `fct_subscriptions(PP)` WHERE STR_TO_DATE(end_date, '%d/%m/%Y') BETWEEN STR_TO_DATE('01/07/2024', '%d/%m/%Y') AND STR_TO_DATE('30/09/2024', '%d/%m/%Y') GROUP BY tier_name ORDER BY Highesttier DESC LIMIT 1;
```
















