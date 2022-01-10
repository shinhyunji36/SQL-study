# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.aapl_historical_stock_price)
```

## COUNT [🔗](https://mode.com/sql-tutorial/sql-count/)
- `not-null`인 행의 개수만 count 된다. (출력된 값은 not-null values의 개수)
- `count` 질의문 결과의 컬럼명이 기본적으로 `count`로 출력된다. 
  - 따라서 `AS`를 사용하여 어떤 컬럼을 `count`한 것인지 한 눈에 알 수 있도록 하기.
  - 보통 `소문자`와 `_`를 조합해서 어떤 것을 `count`한 컬럼인지 밝힌다.
  - 띄어쓰기를 사용할 거라면, 큰 따옴표(`"`)를 써야한다.

### 문제
> Write a query to count the number of non-null rows in the low column.

```sql
SELECT COUNT(low) AS low
FROM tutorial.aapl_historical_stock_price
```
![image](https://user-images.githubusercontent.com/74661937/148775235-486f961d-a4b1-45a0-b953-0f74ce2ed6a4.png)


### 문제
> Write a query that determines counts of every single column. Which column has the most null values?
 ```sql
SELECT COUNT(year) AS count_of_year,
        COUNT(month) AS count_of_month,
        COUNT(open) AS count_of_open,
        COUNT(high) AS count_of_high, 
        COUNT(low) AS count_of_low, 
        COUNT(close) AS count_of_close, 
        COUNT(volume) AS count_of_volume
FROM tutorial.aapl_historical_stock_price
 ```
![image](https://user-images.githubusercontent.com/74661937/148777815-64246207-70b3-43d2-acad-30c867a7ed3b.png)
  - `high` 컬럼이 가장 null-values가 많다.

