# Mode - SQL Advanced
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다.(tutorial.sf_crime_incidents_2014_01 : 2013년 11월 1일부터 2014년 1월 31일까지의 3개월 동안의 샌프란시스코 범죄 사건에 대한 데이터)
```
## Writing Subqueries in SQL 02 [🔗](https://mode.com/sql-tutorial/sql-sub-queries/)

### 조건 로직에서의 서브 쿼리 사용
- `FROM`뿐만 아니라 조건 로직(`WHERE`, `JOIN`/`ON`, `CASE`)에서도 서브쿼리를 사용할 수 있다.

- **`WHERE`과 서브쿼리를 사용할 때**
  - 서브 쿼리가 **단일 셀** 결과를 출력하는 경우 작동한다.
    ```sql
    SELECT *
    FROM tutorial.sf_crime_incidents_2014_01
    WHERE Date = (SELECT MIN(date)
                   FROM tutorial.sf_crime_incidents_2014_01 )
    ```
    ![image](https://user-images.githubusercontent.com/74661937/151663551-311beea9-a85d-4622-ab80-96ec5dfe27ef.png)


    ```sql
    SELECT MIN(date)
    FROM tutorial.sf_crime_incidents_2014_01
    ```
    ![image](https://user-images.githubusercontent.com/74661937/151663508-63fa1e87-5138-4596-b5fd-f197b7b2954c.png)

  - 그러나! **`IN`을 함께 사용한다면** `WHERE`과 **여러 셀** 결과를 출력하는 서브쿼리를 함께 사용 가능하다.
    - 이 경우에는, `IN`에서 서브쿼리가 테이블이 아닌 개별 값(혹은 값의 집합)으로 으로 처리되기 때문에 별칭을 포함해서는 안된다.
      ```sql
      SELECT *
      FROM tutorial.sf_crime_incidents_2014_01
      WHERE Date IN (SELECT date
                     FROM tutorial.sf_crime_incidents_2014_01
                     ORDER BY date
                     LIMIT 5
                    )
      ```




- **`JOIN`과 서브쿼리를 사용할 때**
  - `WHERE`과 서브쿼리를 사용할 때보다 요구사항이 엄격하지 않다.
     <br>ex) 서브쿼리는 여러 결과를 출력할 수 있다.
    ```SQL
    SELECT incidents.*,
           sub.incidents AS incidents_that_day
      FROM tutorial.sf_crime_incidents_2014_01 incidents
      JOIN ( SELECT date,
              COUNT(incidnt_num) AS incidents
               FROM tutorial.sf_crime_incidents_2014_01
              GROUP BY 1
           ) sub
        ON incidents.date = sub.date
     ORDER BY sub.incidents DESC, time
    ```
    
  - 위 쿼리문의 실행 순서
    1. [서브 쿼리] group by와 count를 통해 date 별로 일어난 사건의 수를 계산한다. → 별칭 `sub`
    2. [외부 쿼리] 서브 쿼리로 실행된 결과인 `sub`를 `date`를 기준으로 기존 테이블인 `incidents`와 내부 조인한다.
    3. [외부 쿼리] 이를 `sub`의 `incidents`(incidents_that_day)를 내림차순 정렬한 후,  이를 바탕으로 `time`을 기준으로 오름 차순 정렬한다.

  ![image](https://user-images.githubusercontent.com/74661937/151663335-12ef27b8-3439-4b7b-84b5-807efffc593a.png)
  ![image](https://user-images.githubusercontent.com/74661937/151663356-03f88ef3-dc15-4430-b5a1-d6e83b714875.png)



