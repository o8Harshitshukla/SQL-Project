# Marketing Spending Analysis

## Problem Description

In this marketing spending analysis, we will examine various marketing metrics to evaluate the effectiveness of marketing campaigns. The goal is to gain insights into the return on marketing investment (ROMI) and the performance of different marketing strategies.

## Data Description

We have access to a dataset containing marketing-related information. Here are the columns in the dataset:

![image](https://github.com/Nasir151/SQL-Projects/assets/94509995/056bc016-1c98-4f6e-b61d-adf209fa9a41)

These columns provide information about spending, impressions, clicks, leads, orders, and revenue for specific marketing campaigns on specific dates.

## Metrics to Calculate

To evaluate the effectiveness of marketing campaigns, we will calculate the following metrics:

![image](https://github.com/Nasir151/SQL-Projects/assets/94509995/e6baf3d2-3055-4fdd-a427-eca6389c4b9a)

  Note: ROMI is the most important metric and it is used as the ultimate way to evaluate if the campaign is good or bad.
  
  -----------------------------------------------------------------------------------------------------------------------------------
#### Task 1: Calculate Click-through rate(CTR), Conversion 1, Conversion 2, Average order value(AOV), Cost per click (CPC), Customer acquisition cost (CAC), ROMI for all records.

Question 1.1: What is CTR for google_wide campaign on February 1, 2021?

Question 1.2: What is Conversion 1 for instagram_tier2 campaign on February 5, 2021?

Question 1.3: What is Conversion 2 for facebook_tier1 campaign on February 9, 2021?

Question 1.4: What is AOV for facebook_lal campaign on February 13, 2021?

Question 1.5: What is CPC for facebook_retargeting campaign on February 23, 2021?

Question 1.6: What is CAC for the youtube_blogger campaign on February 24, 2021?

Question 1.7: What is ROMI for the banner_partner campaign on February 28, 2021?

```sql
SELECT *,
  (clicks / impressions) * 100 AS CTR,
  (leads / clicks) AS Conv_1,
  (orders / leads) AS Conv_2,
  (revenue / orders) AS AOV,
  (mark_spent / clicks) AS CPC,
  (mark_spent / orders) AS CAC,
  (revenue - mark_spent) / mark_spent AS ROMI
FROM marketing
```

#### Task 2: Calculate Overall ROMI

```sql
SELECT ( Sum(revenue) - Sum(mark_spent) ) / Sum(mark_spent)
FROM marketing
```

#### Task 3: Calculate ROMI for instagram_blogger campaign between 15th Feb to 25th Feb

```sql
SELECT ((SUM(revenue) - SUM(mark_spent)) / SUM(mark_spent))
FROM marketing
WHERE campaign_name = "instagram_blogger"
AND c_date >= '2021-02-15'
AND c_date <= '2021-02-25'
```

#### Task 4: Find out the average revenue spent on weekend(Saturday and Sunday)

```sql
SELECT Avg(revenue)
FROM marketing
WHERE Dayname(c_date) IN ( "sunday", "saturday" )
```

#### Task 5: Find out the average revenue spent on weekdays(Monday to Friday) with NOT IN

```sql
SELECT Avg(revenue)
FROM marketing
WHERE Dayname(c_date) NOT IN ( "sunday", "saturday"
```

#### Task 6: Which campaign showed the worst loss in a single day? By loss we mean negative gross profit (revenue - marketing spending). Enter campaign ID.

```sql
SELECT campaign_name,
       campaign_id,
       mark_spent,
       impressions,
       c_date,
       leads,
       revenue - mark_spent AS gross_profit
FROM marketing
ORDER BY gross_profit,
         impressions DESC;
```

#### Task 7: How much total money we spent on Facebook on campaigns with negative ROMI.
    Note: Use LOCATE function to find the substring in the string.

```sql
SELECT Sum(mark_spent)
FROM marketing
WHERE ( revenue - mark_spent ) / mark_spent < 0
AND Locate('facebook', campaign_name)
```
