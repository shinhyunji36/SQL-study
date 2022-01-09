# Mode - SQL Basic
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.billboard_top_100_year_end)
```

## SQL Logical Operators - NOT [🔗](https://mode.com/sql-tutorial/sql-not-operator/)

### 주의점 
- Using NOT with < and > usually doesn't make sense.
- For example, this query will return an error.
  ```sql
  /*에러 발생 쿼리*/
  SELECT * 
  FROM tutorial.billboard_top_100_year_end 
  WHERE year = 2013 
  AND year_rank NOT > 3 --에러 발생 구간
  ```
  
- `NOT`은 주로 `LIKE`와 쓰인다.
  ```sql
  SELECT *
	FROM tutorial.billboard_top_100_year_end
	WHERE year = 2013
  AND "group" NOT ILIKE '%macklemore%
  ```
  
- `NOT`은 자주 `NOT NULL` 행을 확인하는데 쓰인다.
  ```SQL
  SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE year = 2013
  AND song_name IS NOT NULL -- 이 경우, IS가 NOT 전에 와야한다.
  ```
  
  
