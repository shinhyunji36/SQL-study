# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (Crunchbase data)
```
## SQL Joins with Comparison Operators [🔗](https://mode.com/sql-tutorial/sql-join-comparison-operators/)
### ON & AND
> join only investments that occurred more than 5 years after each company's founding year <br>애초에 `JOIN`할 때부터 `ON`과 `AND` 조건에 맞는 행들만 가져와서 결합한다.
  ```sql
  SELECT companies.permalink,
         companies.name,
         companies.status,
         COUNT(investments.investor_permalink) AS investors
    FROM tutorial.crunchbase_companies companies
    LEFT JOIN tutorial.crunchbase_investments_part1 investments
      ON companies.permalink = investments.company_permalink
     AND investments.funded_year > companies.founded_year + 5
   GROUP BY 1, 2, 3
  ``` 
  ![image](https://user-images.githubusercontent.com/74661937/150294033-d810cb6d-feb7-4349-847c-88168a46ab36.png)

### ON & WHERE
> 모든 행에 대해 `JOIN` 한 이후, `WHERE` 조건에 맞게 결과를 필터링한다. <br>(따라서 위의 쿼리와 다른 결과를 나타낸다.)
```sql
SELECT companies.permalink,
       companies.name,
       companies.status,
       COUNT(investments.investor_permalink) AS investors
  FROM tutorial.crunchbase_companies companies
  LEFT JOIN tutorial.crunchbase_investments_part1 investments
    ON companies.permalink = investments.company_permalink
 WHERE investments.funded_year > companies.founded_year + 5
 GROUP BY 1, 2, 3
```
![image](https://user-images.githubusercontent.com/74661937/150295106-279eabbb-c4ab-4b29-96d3-69fe75dbcaec.png)
