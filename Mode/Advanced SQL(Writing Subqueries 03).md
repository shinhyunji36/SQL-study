# Mode - SQL Advanced
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다.(Crunchbase)
```
## Writing Subqueries in SQL 03 [🔗](https://mode.com/sql-tutorial/sql-sub-queries/)

### 서브쿼리와 UNION
- `UNION`으로 합쳐질 각각의 데이터 세트에 먼저 집계를 하는 것이 아니라, <br>`UNION`으로 합쳐진 전체 데이터 세트에 대해 집계를 하려고 할 때 `FROM`의 서브쿼리를 작성하면 된다.
  ```sql
  SELECT COUNT(*) AS total_rows
  FROM (
          SELECT *
            FROM tutorial.crunchbase_investments_part1

           UNION ALL

          SELECT *
            FROM tutorial.crunchbase_investments_part2
         ) sub
  ```
  ![image](https://user-images.githubusercontent.com/74661937/151702119-6ea24464-c94e-4366-ab44-786484bb15d0.png)
  
- 문제
  > Write a query that ranks investors from the combined dataset above by the total number of investments they have made.


  ```SQL
  SELECT sub.investor_name,
      COUNT(*) AS count_investor
  FROM (
        SELECT *
          FROM tutorial.crunchbase_investments_part1

         UNION ALL

        SELECT *
          FROM tutorial.crunchbase_investments_part2
       ) sub
  GROUP BY 1
  ORDER BY 2 DESC
  ```
  ![image](https://user-images.githubusercontent.com/74661937/151702511-317f6317-a938-4e66-bcd2-96fd90bddfc5.png)



- 문제 2 (틀린 문제)
 > Write a query that does the same thing as in the previous problem, except only for companies that are still operating. <br>Hint: operating status is in tutorial.crunchbase_companies.
  
 - 나의 오답 
  ```sql
  SELECT sub.investor_name,
         COUNT(*) AS count_investors
  FROM (
        SELECT *
          FROM tutorial.crunchbase_investments_part1

         UNION ALL

        SELECT *
          FROM tutorial.crunchbase_investments_part2
       ) sub

  JOIN tutorial.crunchbase_companies companies
  ON companies.permalink = sub.company_permalink
  
  WHERE companies.status != 'operating'
  GROUP BY 1
  ORDER BY 2 DESC 
  ```
  ![image](https://user-images.githubusercontent.com/74661937/151702897-215149fd-b5e0-41d3-a158-50898e82ef97.png)

  
  - 답
    ```sql
    SELECT investments.investor_name,
       COUNT(investments.*) AS investments
    FROM tutorial.crunchbase_companies companies
    JOIN (
        SELECT *
          FROM tutorial.crunchbase_investments_part1
         
         UNION ALL
        
         SELECT *
           FROM tutorial.crunchbase_investments_part2
         ) investments
    ON investments.company_permalink = companies.permalink
    
    WHERE companies.status = 'operating'
    GROUP BY 1
    ORDER BY 2 DESC
    ```
  ![image](https://user-images.githubusercontent.com/74661937/151702936-567fe08c-84b3-458c-a627-90bd465354ae.png)


  - 문제에서 `status`가 `operating`인 경우를 제외하고 카운트하라고 했는데, 그러기 위해서는 일단 `status` 컬럼에 값이 있어야 한다. <br>(operating / closed / ...와 같은 값을 갖고 있는 `status` 컬럼)
  - 이를 위해서는 status 컬럼이 있는 cruchbase_companies 테이블을 먼저 가져온 후, <br>해당 테이블에 있는 `company_permalink`를 기준으로 `UNION ALL`을 통해 합친 서브 쿼리의 결과 테이블을 내부 조인해야한다.





