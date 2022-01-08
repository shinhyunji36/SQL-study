# Mode - SQL Basic
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.billboard_top_100_year_end)
```

## SQL Logical Operators - IS NULL [🔗](https://mode.com/sql-tutorial/sql-is-null/)

> artist 컬럼에서 null값을 가진 행 모두 불러오기

```SQL
SELECT * FROM tutorial.billboard_top_100_year_end WHERE artist IS NULL
```

### 주의점
- `WHERE artist = NULL`은 실행되지 않는다. 
- NULL 값에 산수(`=`)를 수행할 수 없기 때문. 
- 반드시 `IS NULL`을 써줘야 한다. 
