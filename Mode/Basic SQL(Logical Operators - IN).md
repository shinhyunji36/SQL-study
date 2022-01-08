# Mode - SQL Basic
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.billboard_top_100_year_end)
```

## SQL Logical Operators - IN [🔗](https://mode.com/sql-tutorial/sql-in-operator/)

### 문제
> Write a query that shows all of the entries for Elvis and M.C. Hammer.

- 힌트
  ```
  M.C. Hammer is actually on the list under multiple names, 
  so you may need to first write a query to figure out exactly how M.C. Hammer is listed.
  ```
  
### 풀이 과정 및 오답

- 풀이 과정

  1. 힌트를 확인하고 `Hammer`의 이름들 확인
      ```sql
      SELECT *
      FROM tutorial.billboard_Top_100_year_end
      WHERE "group" ILIKE '%hammer%'
      ```
      ![image](https://user-images.githubusercontent.com/74661937/148643119-de7789a0-9746-4e82-9161-7029c65422d4.png)

  2. 그 후 Elvis와 M.C. Hammer의 데이터를 조회하기 위해 쿼리 작성
      ```sql
      SELECT * 
      FROM tutorial.billboard_Top_100_year_end
      WHERE "group" IN ('%Hammer', 'Elvis%)
      ```
    
- 오답 이유
  1) `Hammer`가 `M.C. Hammer`, `Hammer`로 저장되어 있음을 확인하고, `IN`의 괄호 안에 확실한 이름을 지정해주어야 한다.
  2) 와일드카드(%)는 LIKE, ILIKE와 함께 쓰는 것이지 범위를 지정하는 IN과는 사용 X
  3) `Elvis`도 정확한 이름(full name)이 무엇인지 확인하고 `IN`의 괄호 안에 써주어야 한다.


### 답
  ```SQL
  SELECT *
  FROM tutorial.billboard_top_100_end
  WEHRE "group" IN ('M.C. Hammer', 'Hammer', 'Elvis Presley')
  ```
