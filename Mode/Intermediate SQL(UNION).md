# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (Crunchbase data)
```
## UNION [🔗](https://mode.com/sql-tutorial/sql-union/)
- `UNION` allows you to **stack** one dataset on top of the other.
  <br>cf. `Joins` allow you to **combine two datasets side-by-side.**
  ```sql
  SELECT *
  FROM tutorial.crunchbase_investments_part1
  
  UNION

  SELECT *
  FROM tutorial.crunchbase_investments_part2
  ```
  
- `UNION`은 기본적으로 `DISTINCT` 값만 추가한다.
  - 첫번째 `SELECT`절에서 가져온 테이블1의 행들은 모두 보여주되, 두번째 `SELECT`절에서 가져온 테이블2에서 중복되는 값은 제거하고 보여준다.
- 중복값을 삭제하지 않고 추가하기 위해서는 `UNION ALL`을 사용한다.
- `SELECT`할 두 테이블 간 중복되는 행이 확실히 없는 경우, 
  - `UNION`은 두번째 테이블에 중복되는 행이 있는지 검토하는 과정을 거쳐야하기때문에 좀 더 처리 시간이 오래 걸릴 수 있다.
  - 따라서 확실히 중복행이 없을 땐, 이런 검토 과정을 거치지 않고 바로 테이블2를 STACK 해버리는 `UNION ALL`을 사용하는 것도 좋은 방법이다. (처리 시간 단축을 위해서)
- `SELECT`절을 여러번 쓰기 때문에 `UNION`으로 테이블들을 결합하기 전에 `WHERE`로 각 테이블을 필터링할 수 있다.


### 데이터 추가시 유의 사항
1. 두 테이블에 동일한 수의 열이 있어야 한다.
2. 두번째 테이블의 열은 첫번째 테이블과 동일한 순서로 동일한 데이터 유형을 가져야 한다. <br>(열 이름이 완전히 동일할 필요는 X)

### 문제
> Write a query that appends the two crunchbase_investments datasets above (including duplicate values). <br> Filter the first dataset to only companies with names that start with the letter "T", and filter the second to companies with names starting with "M" (both not case-sensitive). <br> Only include the company_permalink, company_name, and investor_name columns.

```sql
SELECT company_permalink, 
       company_name,
       investor_name
FROM tutorial.crunchbase_investments_part1 
WHERE company_name ILIKE 'T%'

UNION ALL

SELECT company_permalink,
       company_name, 
       investor_name
FROM tutorial.crunchbase_investments_part2
WHERE company_name ILIKE 'M%'
```
- 중복값 포함 : `UNION ALL`
-  `UNION`은 각각의 테이블에 `SELECT`절을 따로 쓰기 때문에, `JOIN`처럼 FROM에 따로 별칭을 지정해주어 하나하나 참조하지 않아도 됨.


