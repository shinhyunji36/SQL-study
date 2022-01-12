# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.aapl_historical_stock_price)
```

## HAVING [🔗](https://mode.com/sql-tutorial/sql-having/)
- `GROUP BY`로 월별 집계 통계를 알 수 있게 되었습니다. 
  - 그 다음 단계로 월별 집계 통계가 `특정 값 이상`인 데이터를 찾고 싶습니다. 
  - 이 경우, `WHERE`은 사용할 수 없습니다. (`WHERE`은 집계 컬럼에 대하여 필터링을 하지 않기 때문)
- **집계 컬럼에 대하여 조건(필터)을 주고 싶을 때 사용하는 것**이 `HAVING`입니다.
- 주로 서브쿼리로 사용됩니다.

```SQL
SELECT year,
       month,
       MAX(high) AS month_high
FROM tutorial.aapl_historical_stock_price
GROUP BY year, month
HAVING MAX(high) > 400
ORDER BY year, month
```
![image](https://user-images.githubusercontent.com/74661937/149051246-ee3e4497-494a-4a64-a548-cc7263b1d919.png)



---
### 쿼리문 순서
1. `SELECT`
2. `FROM`
3. `WHERE`
4. `GROUP BY`
5. `HAVING`
6. `ORDER BY`
