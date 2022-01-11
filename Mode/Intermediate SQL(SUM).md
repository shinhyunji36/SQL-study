# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.aapl_historical_stock_price)
```

## SUM [🔗](https://mode.com/sql-tutorial/sql-sum/)
- 오적 `numerical values`를 가진 컬럼에만 적용 가능
- 오직 `수직적` 집계만 가능
  - 행에 대한 `수평적 집계`를 원한다면, [간단한 산술](https://mode.com/sql-tutorial/sql-operators/#arithmetic-in-sql)을 사용하여 수행해야 한다.
- `null`을 `0`으로 취급한다.


### 문제
> Write a query to calculate the average opening price.<br>
  (hint: you will need to use both COUNT and SUM, as well as some simple arithmetic.)

### 답
```sql
SELECT SUM(open) / COUNT(open) AS average_of_opening_price
FROM tutorial.aapl_historical_stock_price
```
![image](https://user-images.githubusercontent.com/74661937/148918915-3c6d3c0b-6602-4e4f-b028-c0f459e4c0e0.png)
