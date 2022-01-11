# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.aapl_historical_stock_price)
```

## GROUP BY [🔗](https://mode.com/sql-tutorial/sql-group-by/)
- `COUNT`, `AVG`, `SUM`과 같은 SQL 집계 함수를 사용할 때, 각 카테고리 별로 집계를 하고 싶은 경우 사용한다.
  ```sql
  SELECT year,
        COUNT(*) AS count
  FROM tutorial.aapl_historical_stock_price
  GROUP BY year
  ```
  ![image](https://user-images.githubusercontent.com/74661937/148924855-11e70d02-eac4-4c0a-9030-403150fed096.png)


- 여러 컬럼에 대해서도 `GROUP BY`를 사용할 수 있다.
  - 이 경우에는, `,`로 컬럼 이름을 분리시켜 나열해야 한다.
  ```SQL
  SELECT year,
          month,
          COUNT(*) AS count
  FROM tutorial.aapl_historical_stock_price
  GROUP BY year, month
  ```
  ![image](https://user-images.githubusercontent.com/74661937/148925283-d294378d-93a5-4ea4-8ab1-7f66f7e1c3d1.png) 

### 문제
> Calculate the total number of shares traded each month. <br> Order your results chronologically.
  - total number of shares = `SUM(volume)`
  - each month = `year`, `month`

### 답
```sql
SELECT year, 
      month,
      SUM(volume) AS volumne_sum
  FROM tutorial.aapl_historical_stock_price
 GROUP BY year, month
 ORDER BY year, month
 ```
 
### ORDER BY와 GROUP BY
- `GROUP BY`에서 컬럼의 순서는 중요하지 않다. 어떤 컬럼이 먼저 쓰였든 간에, 결과는 동일하게 나온다.
  - 따라서 `GROUP BY`가 적용되는 컬럼을 바꾸고 싶다면, `ORDER BY`를 함께 사용해야 한다.
  - **CASE 1** : ORDER BY year, month
    ```SQL
    SELECT year, 
          month,
          COUNT(*) AS count
    FROM tutorial.aapl_historical_stock_price
    GROUP BY year, month
    ORDER BY year, month
    ```
    ![image](https://user-images.githubusercontent.com/74661937/148927631-62c9846e-64ed-4c28-9c67-76c4208f35c4.png)

   - **CASE 2** : ORDER BY month, year
     ```SQL
     SELECT year, 
            month,
            COUNT(*) AS count
     FROM tutorial.aapl_historical_stock_price
     GROUP BY year, month
     ORDER BY month, year
     ```
     ![image](https://user-images.githubusercontent.com/74661937/148927860-e7eefa0c-3f24-4666-a0dd-7aa5bff8fc15.png)
 
 
 ### LIMIT와 GROUP BY
 - SQL은 `LIMIT`보다 집계를 먼저 실행한다.
 - 집계 함수 실행 후, 그 중 LIMIT 값 만큼의 행만 출력한다.



 
 
