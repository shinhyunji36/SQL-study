# Mode - SQL Advanced
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.crunchbase_companies_clean_date, tutorial.crunchbase_acquisitions_clean_date)
```
## Date Format [🔗](https://mode.com/sql-tutorial/sql-datetime-format/)
- `INTERVAL` : 시간 간격 도입시 사용
  - `INTERVAL '1 week'`
  - `INTERVAL '10 seconds'`
  - `INTERVAL '5 months`'
  
  ```SQL
  SELECT companies.permalink,
         companies.founded_at_clean,
         companies.founded_at_clean::timestamp +
         INTERVAL '1 week' AS plus_one_week
  FROM tutorial.crunchbase_companies_clean_date companies
  WHERE founded_at_clean IS NOT NULL
  ```
  ![image](https://user-images.githubusercontent.com/74661937/150805608-b6902397-191f-460c-922d-16007d1df1a2.png)

  
- `NOW()` : 현재 시간을 추가할 때 사용
  ```SQL
  SELECT companies.permalink,
       companies.founded_at_clean,
       NOW() - companies.founded_at_clean::timestamp AS founded_time_ago
  FROM tutorial.crunchbase_companies_clean_date companies
  WHERE founded_at_clean IS NOT NULL
  ```
  ![image](https://user-images.githubusercontent.com/74661937/150806596-03fd74a7-f623-42de-84ef-3d73ede5f3eb.png)

## 문제
> Write a query that counts the number of companies acquired within 3 years, 5 years, and 10 years of being founded **(in 3 separate columns)**. <br>Include **a column for total companies acquired** as well. <br>**Group by category** and limit to only rows with a founding date.

```SQL
SELECT companies.category_code, --for group by category
       COUNT(CASE WHEN acquisitions.acquired_at_cleaned <= companies.founded_at_clean::timestamp + INTERVAL '3 years' 
                       THEN 1 ELSE NULL END) AS acquired_3_yrs,
       COUNT(CASE WHEN acquisitions.acquired_at_cleaned <= companies.founded_at_clean::timestamp + INTERVAL '5 years'
                       THEN 1 ELSE NULL END) AS acquired_5_yrs,
       COUNT(CASE WHEN acquisitions.acquired_at_cleaned <= companies.founded_at_clean::timestamp + INTERVAL '10 years'
                       THEN 1 ELSE NULL END) AS acquired_10_yrs, --counts the number of companies acquired within 3 years, 5 years
       COUNT(1) AS total --a column for total companies acquired
  FROM tutorial.crunchbase_companies_clean_date companies
  JOIN tutorial.crunchbase_acquisitions_clean_date acquisitions
    ON acquisitions.company_permalink = companies.permalink
 WHERE founded_at_clean IS NOT NULL --limit to only rows with a founding date
 GROUP BY 1
 ORDER BY 5 DESC
 ```
 ![image](https://user-images.githubusercontent.com/74661937/150808494-2f73e921-dae5-46c1-b151-865a223e5e28.png)


  
