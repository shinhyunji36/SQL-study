# Mode - SQL Advanced
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다.(tutorial.sf_crime_incidents_2014_01 : 2013년 11월 1일부터 2014년 1월 31일까지의 3개월 동안의 샌프란시스코 범죄 사건에 대한 데이터)
```
## Writing Subqueries in SQL 01 [🔗](https://mode.com/sql-tutorial/sql-sub-queries/)

### 서브 쿼리(basic)
- 서브 쿼리 및 내부 쿼리
- 예시 쿼리문 (서브쿼리는 `FROM` 문의 (괄호) 안 쿼리)
  ```sql
  SELECT sub.*
  FROM (
        SELECT *
          FROM tutorial.sf_crime_incidents_2014_01
         WHERE day_of_week = 'Friday'
       ) sub
  WHERE sub.resolution = 'NONE'
  ```
  ![image](https://user-images.githubusercontent.com/74661937/151563588-3df83022-beca-4605-9851-a25c797010ef.png)



- 데이터베이스가 내부 쿼리를 독립 쿼리로 처리해야하기 때문에 서브쿼리는 자체적으로 실행 가능해야 한다.
- 서브 쿼리가 실행된 결과가 기본 테이블이 되어 외부 쿼리가 실행된다.
  ```sql
  SELECT sub.*
  FROM (
       <<results from inner query go here>>
       ) sub
  WHERE sub.resolution = 'NONE'
  ```
  
- 서브 쿼리에는 이름이 있어야 한다. 별칭 추가 필요 (`sub`) 


### 서브 쿼리를 사용해 여러 단계에서 집계
>  각 요일에 보고되는 사건의 수 파악하기<br>12월의 금요일에 평균적으로 얼마나 많은 사고가 발생하는지 파악하기
- `서브 쿼리`로 매일 사고 수를 계산한 후, 이를 기본 테이블로 삼아 `외부 쿼리`로 월별 평균을 집계합니다.
  ```sql
    SELECT LEFT(sub.date, 2) AS cleaned_month,
           sub.day_of_week, 
           AVG(sub.incidents) AS average_incidents
    FROM (
          SELECT day_of_week,
                 date,
                 COUNT(incidnt_num) AS incidents
          FROM tutorial.sf_crime_incidents_2014_01
          GROUP BY 1, 2
        ) sub
  GROUP BY 1, 2
  ORDER BY 1,2
  ```
  
  - 서브 쿼리 실행 결과
  
    ![image](https://user-images.githubusercontent.com/74661937/151564803-8dbb96a1-b716-44ca-81a8-e9d78964db9d.png)

    
  - 전체 쿼리 실행 결과
  
    ![image](https://user-images.githubusercontent.com/74661937/151565475-06618a18-ea94-448a-bcba-37fea67a9032.png)

    
    
    


  
