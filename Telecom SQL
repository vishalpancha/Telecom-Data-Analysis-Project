
#1)Total Revenue

SELECT 
    ROUND(SUM(itv_revenue_crores), 2) AS total_revenue_crores
FROM
    fact_itv_metrics;

#2)Avg Revenue 

SELECT 
    ROUND(AVG(itv_revenue_crores), 2) AS avg_revenue_crores
FROM
    fact_itv_metrics;

#3)Avg of arpu

SELECT 
    AVG(arpu) AS Avg_revenue_per_user
FROM
    fact_itv_metrics;

#4)Total Active Users 

SELECT 
    ROUND(SUM(active_users_lakhs), 2) AS Total_active_users_lakhs
FROM
    fact_itv_metrics;

#5)Total Unsubscribed Users

SELECT 
    ROUND(SUM(unsubscribed_users_lakhs), 2) AS Total_Unsubscribed_Users_lakhs
FROM
    fact_itv_metrics;

#6)Average of active users per month

SELECT 
    month_name,
    ROUND(AVG(active_users_lakhs), 2) AS active_users_per_month_lakh
FROM
    fact_itv_metrics
        JOIN
    dim_date ON SUBSTRING_INDEX(dim_date.date, '0', - 1) = fact_itv_metrics.date
GROUP BY month_name;

#7)Average of ms_pct

SELECT 
    ROUND(AVG(ms_pct)) AS Market_Share_percentage
FROM
    fact_market_share;

#8)Total Revenue for all periods before 5G implementation

SELECT 
    time_period, ROUND(SUM(itv_revenue_crores), 2)
FROM
    fact_itv_metrics
        JOIN
    dim_date ON fact_itv_metrics.date = SUBSTRING_INDEX(dim_date.date, '0', - 1)
WHERE
    dim_date.`before/after_5G` = 'Before 5G'
GROUP BY time_period;

#9)Total Revenue for all periods after 5G implementation

SELECT 
    time_period, ROUND(SUM(itv_revenue_crores), 2)
FROM
    fact_itv_metrics
        INNER JOIN
    dim_date ON fact_itv_metrics.date = SUBSTRING_INDEX(dim_date.date, '0', - 1)
WHERE
    dim_date.`before/after_5G` = 'After 5G'
GROUP BY time_period;

#10)ARPU of periods before 5G implementation

SELECT 
    time_period, AVG(arpu)
FROM
    fact_itv_metrics
        INNER JOIN
    dim_date ON fact_itv_metrics.date = SUBSTRING_INDEX(dim_date.date, '0', - 1)
WHERE
    dim_date.`before/after_5G` = 'Before 5G'
GROUP BY time_period;

#11)ARPU of periods after 5G implementation

SELECT 
    time_period, AVG(arpu)
FROM
    fact_itv_metrics
        INNER JOIN
    dim_date ON fact_itv_metrics.date = SUBSTRING_INDEX(dim_date.date, '0', - 1)
WHERE
    dim_date.`before/after_5G` = 'After 5G'
GROUP BY time_period;

#12)Total active users for all periods before 5G implementation

SELECT 
    time_period, ROUND(SUM(active_users_lakhs), 2)
FROM
    fact_itv_metrics
        INNER JOIN
    dim_date ON fact_itv_metrics.date = SUBSTRING_INDEX(dim_date.date, '0', - 1)
WHERE
    dim_date.`before/after_5G` = 'Before 5G'
GROUP BY time_period;

#13)Total active users for all periods after 5G implementation

SELECT 
    time_period, ROUND(SUM(active_users_lakhs), 2)
FROM
    fact_itv_metrics
        INNER JOIN
    dim_date ON fact_itv_metrics.date = SUBSTRING_INDEX(dim_date.date, '0', - 1)
WHERE
    dim_date.`before/after_5G` = 'After 5G'
GROUP BY time_period;

#14)Total unsubscribed users for all periods before 5G implementation

SELECT 
    time_period, ROUND(SUM(unsubscribed_users_lakhs), 2)
FROM
    fact_itv_metrics
        INNER JOIN
    dim_date ON fact_itv_metrics.date = SUBSTRING_INDEX(dim_date.date, '0', - 1)
WHERE
    dim_date.`before/after_5G` = 'Before 5G'
GROUP BY time_period;

#15)Total unsubscribed users for all periods before 5G implementation

SELECT 
    time_period, ROUND(SUM(unsubscribed_users_lakhs), 2)
FROM
    fact_itv_metrics
        INNER JOIN
    dim_date ON fact_itv_metrics.date = SUBSTRING_INDEX(dim_date.date, '0', - 1)
WHERE
    dim_date.`before/after_5G` = 'After 5G'
GROUP BY time_period;

# 16)Percentage growth in revenue before and after 5g implementation for each city

WITH RevenueByPeriod AS (
    SELECT 
        dim_cities.city_name,
        SUM(CASE WHEN dim_date.`before/after_5g` = 'before 5g' THEN fact_itv_metrics.itv_revenue_crores ELSE 0 END) AS revenue_before_5g,
        SUM(CASE WHEN dim_date.`before/after_5g` = 'after 5g' THEN fact_itv_metrics.itv_revenue_crores ELSE 0 END) AS revenue_after_5g
    FROM 
        fact_itv_metrics
    JOIN 
        dim_date ON fact_itv_metrics.date =  substring_index(dim_date.date, 0, -1)
    JOIN 
        dim_cities ON fact_itv_metrics.city_code = dim_cities.city_code
    GROUP BY 
        dim_cities.city_name
)
SELECT 
    city_name,
    revenue_before_5g,
    revenue_after_5g,
    ROUND(
        (revenue_after_5g - revenue_before_5g) / NULLIF(revenue_before_5g, 0) * 100,
        2
    ) AS percentage_growth
FROM 
    RevenueByPeriod;
    
# 17)Percentage growth in revenue before and after 5g implementation for each Plan  
  
WITH RevenueByPeriod AS (
    SELECT 
        dim_plan.plan,
        SUM(CASE WHEN dim_date.`before/after_5g` = 'before 5g' THEN plan_revenue_crores ELSE 0 END) AS revenue_before_5g,
        SUM(CASE WHEN dim_date.`before/after_5g` = 'after 5g' THEN plan_revenue_crores ELSE 0 END) AS revenue_after_5g
    FROM 
        fact_plan_revenue
    JOIN 
        dim_date ON fact_plan_revenue.date =dim_date.date
    JOIN 
        dim_plan ON fact_plan_revenue.plans = dim_plan.plan
    GROUP BY 
        dim_plan.plan
)
SELECT 
   plan,
   revenue_before_5g,
   revenue_after_5g,
    ROUND(
        (revenue_after_5g - revenue_before_5g) / NULLIF(revenue_before_5g, 0) * 100,
        2
    ) AS percentage_growth
FROM 
    RevenueByPeriod;
    
# 18)City wise active users in lakh before 5g and after 5g 

with right_table as
(select city_name, `before/after_5g`, sum(active_users_lakhs) 
from fact_itv_metrics 
join dim_cities
on fact_itv_metrics.city_code = dim_cities.city_code
join dim_date on fact_itv_metrics.date = substring_index(dim_date.date, '0', -1) group by city_name  , 
`before/after_5g`)  

select city_name ,
sum(case when  `before/after_5g` = 'Before 5G'  then `sum(active_users_lakhs)`  end) as before_5g,
sum(case when  `before/after_5g` = 'After 5G'  then `sum(active_users_lakhs)` end) as After_5g
from   right_table  group by city_name;
    
# 19)Market share over months for all companies

SELECT 
    month_name,
    AVG(CASE
        WHEN fact_market_share.`company` = 'itv' THEN ms_pct
    END) AS itv_marketshare,
    AVG(CASE
        WHEN fact_market_share.`company` = 'Aiirtel' THEN ms_pct
    END) AS Aiirtel_marketshare,
    AVG(CASE
        WHEN fact_market_share.`company` = 'Lio' THEN ms_pct
    END) AS Lio_marketshare,
    AVG(CASE
        WHEN fact_market_share.`company` = 'DADAFONE' THEN ms_pct
    END) AS DADAFONE_marketshare,
    AVG(CASE
        WHEN fact_market_share.`company` = 'Others' THEN ms_pct
    END) AS Others_marketshare
FROM
    fact_market_share
        JOIN
    dim_date ON SUBSTRING_INDEX(dim_date.date, '0', - 1) = fact_market_share.date
GROUP BY month_name;

# 20)Top Plans by revenue

SELECT 
    plans, SUM(plan_revenue_crores) AS revenue
FROM
    fact_plan_revenue
GROUP BY plans;

# 21)Monthly trend of avg ARPU before and after 5g

SELECT 
    month_name,
    AVG(CASE
        WHEN `before/after_5g` = 'Before 5G' THEN arpu
    END) AS Avg_arpu_before_5g,
    AVG(CASE
        WHEN `before/after_5g` = 'After 5G' THEN arpu
    END) AS Avg_arpu_after_5g
FROM
    fact_itv_metrics
        JOIN
    dim_date ON fact_itv_metrics.date = SUBSTRING_INDEX(dim_date.date, '0', - 1)
GROUP BY dim_date.month_name;
 
# 22)Monthly trend of avg active users before and after 5g

SELECT 
    month_name,
    AVG(CASE
        WHEN `before/after_5G` = 'Before 5G' THEN active_users_lakhs
    END) AS avg_activeuser_before_5g,
    AVG(CASE
        WHEN `before/after_5G` = 'After 5G' THEN active_users_lakhs
    END) AS avg_activeuser_before_5g
FROM
    fact_itv_metrics
        JOIN
    dim_date ON fact_itv_metrics.date = SUBSTRING_INDEX(dim_date.date, '0', - 1)
GROUP BY dim_date.month_name;

# 23)Monthly trend of unsubscribed users before 5g and after 5g

SELECT 
    month_name,
    SUM(CASE
        WHEN `before/after_5G` = 'Before 5G' THEN unsubscribed_users_lakhs
    END) AS unsubUser_before_5g,
    SUM(CASE
        WHEN `before/after_5G` = 'After 5G' THEN unsubscribed_users_lakhs
    END) AS unsubUser_after_5g
FROM
    fact_itv_metrics
        JOIN
    dim_date ON fact_itv_metrics.date = SUBSTRING_INDEX(dim_date.date, '0', - 1)
GROUP BY month_name; 
    
