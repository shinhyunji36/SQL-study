# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (Crunchbase data)
```
## FULL OUTER JOIN [🔗](https://mode.com/sql-tutorial/sql-full-outer-join/)
- MySQL에서는 `FULL OUTER JOIN`구문을 사용할 수 없다. 
  - 대신, 
    `FULL OUTER JOIN`이 `LEFT JOIN`과 `RIGHT JOIN` 결과의 합집합이라는 점을 이용하여 같은 결과를 낼 수 있다. (`UNION` 활용)

> EXAMPLE
```SQL
SELECT COUNT(CASE WHEN companies.permalink IS NOT NULL AND acquisitions.company_permalink IS NULL
                  THEN companies.permalink ELSE NULL END) AS companies_only,
       COUNT(CASE WHEN companies.permalink IS NOT NULL AND acquisitions.company_permalink IS NOT NULL
                  THEN companies.permalink ELSE NULL END) AS both_tables,
       COUNT(CASE WHEN companies.permalink IS NULL AND acquisitions.company_permalink IS NOT NULL
                  THEN acquisitions.company_permalink ELSE NULL END) AS acquisitions_only
FROM tutorial.crunchbase_companies companies
FULL JOIN tutorial.crunchbase_acquisitions acquisitions
ON companies.permalink = acquisitions.company_permalink
```
![image](https://user-images.githubusercontent.com/74661937/150099845-2e2a2192-3a06-4120-8149-9be9b42e3e96.png)


### 문제
> tutorial.crunchbase_companies을 tutorial.crunchbase_investments_part1사용하여 조인하는 쿼리를 작성합니다 FULL JOIN. 위의 예와 같이 일치/비매칭 행의 수를 세십시오.

### 답
```SQL
SELECT COUNT (CASE WHEN companies.permalink IS NOT NULL AND investments.company_permalink IS NULL
                   THEN companies.permalink ELSE NULL END) AS companies_only,
       COUNT(CASE WHEN companies.permalink IS NOT NULL AND investments.company_permalink IS NOT NULL
                  THEN companies.permalink ELSE NULL END) AS both_tables,
       COUNT(CASE WHEN companies.permalink IS NULL AND investments.company_permalink IS NOT NULL
                  THEN investments.company_permalink ELSE NULL END) AS investments_only
FROM tutorial.crunchbase_companies companies
FULL JOIN tutorial.crunchbase_investments_part1 investments
ON companies.permalink = investments.company_permalink
```
![image](https://user-images.githubusercontent.com/74661937/150099374-5cb0e0da-fc57-4967-9c5d-0f373810c084.png)


### 헷갈렸던 부분
- `IS NULL`과 `IS NOT NULL` 부분이 어떤 것을 가리키는지 명확하게 이해하지 못해서 각 구문이 왜 `companies_only`, `both_tables`, `investments_only` 영역을 가리키는지 헷갈렸음
- 그래서 어떻게 생각했는가? 실제로 테이블이 어떻게 `FULL JOIN(합집합)` 되는지 생각해보면 쉽게 이해할 수 있다.
  - Table `topic`
    |tid|title|author_id|
    |---|---|---|
    |1|HTML|1|
    |2|CSS|2|
    |3|Database|1|
    |4|Oracle|NULL|
  
  - Table `author`
    |aid|name|city|
    |---|---|---|
    |1|Kim|seoul|
    |2|Jung|jeju|
    |3|Lee|namhae|
    
  **FULL OUTER JOIN** 구문을 사용할 수 있는 경우
  ```sql
  SELECT *
  FROM topic
  FULL OUTER JOIN author
  ON topic.author_id = author.aid
  ```
  
  **FULL OUTER JOIN** 구문을 사용할 수 없는 경우
  ```SQL
  (SELECT * FROM topic LEFT JOIN author ON topic.author_id = author.aid)
  
  UNION
  
  (SELECT * FROM topic RIGHT JOIN author ON topic.author_id = author.aid)
  ```
  
- 중간 과정 (선 `LEFT JOIN` 후 `RIGHT JOIN`)
  |tid|title|author_id|aid|name|city|
  |---|---|---|---|---|---|
  |1|HTML|1|1|Kim|seoul|
  |2|CSS|2|2|Jung|jeju|
  |3|Database|1|1|Kim|seoul|
  |4|Oracle|**NULL**|**NULL**|**NULL**|**NULL**|
  |1|HTML|1|1|Kim|seoul|
  |3|Database|1|1|Kim|seoul|
  |2|CSS|2|2|Jung|jeju|
  |**NULL**|**NULL**|**NULL**|3|Lee|namhae|
  - 여기에서 중복되는 것을 지우면, 결과가 나온다!  <br>(`UNION`의 기본이 **distinct** 이기 때문!)

- 결과
  |tid|title|author_id|aid|name|city|
  |---|---|---|---|---|---|
  |1|HTML|1|1|Kim|seoul|
  |2|CSS|2|2|Jung|jeju|
  |3|Database|1|1|Kim|seoul|
  |4|Oracle|**NULL**|**NULL**|**NULL**|**NULL**|
  |**NULL**|**NULL**|**NULL**|3|Lee|namhae|
  
- 결과 표를 보았을 때, `CASE WHEN companies.permalink IS NOT NULL AND investments.company_permalink IS NULL` 구문은
  - `CASE WHEN companies.permalink IS NOT NULL` : `companies` 테이블에는 값이 있는데, 
  - `AND investments.company_permalink IS NULL` : `investments` 테이블에는 값이 없는 경우.
    - 즉, `companies` 테이블에만 유일하게 존재하는 값을 찾는 것을 알 수 있다.
  
  
  
  
