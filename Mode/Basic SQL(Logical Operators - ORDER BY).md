# Mode - SQL Basic
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.billboard_top_100_year_end)
```

## SQL Logical Operators - ORDER BY [🔗](https://mode.com/sql-tutorial/sql-order-by/)

### CASE 1
  - 1단계 : `year` 컬럼 내림차순 정렬(DESC)
  - 2단계 : `year` 컬럼의 정렬된 각 카테고리 내에서 `year_rank`컬럼에 따라 오름차순 정렬(ASC)

    ```SQL
    SELECT *
    FROM tutorial.billboard_top_100_year_end
    WHERE year_rank <= 3
    ORDER BY year DESC, year_rank
    ```
    ![image](https://user-images.githubusercontent.com/74661937/148685399-e5496012-9cf8-4719-997e-fc4e3f6f7e9f.png)


### CASE 2
- 1단계 : `year_rank` 컬럼 오름차순 정렬(ASC)
- 2단계 : `year_rank` 컬럼의 정렬된 각 카테고리 내에서 `year`컬럼에 따라 내림차순 정렬(DESC)

  ```SQL
  SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE year_rank <= 3
  ORDER BY year_rank, year DESC
  ```
  ![image](https://user-images.githubusercontent.com/74661937/148685509-bc6d61a8-2277-4b0b-95cd-19a4ff4f9579.png)

  

### 주의점
- `DESC`연산자는 그 앞에 오는 컬럼에만 적용된다.
- `ORDER BY` 이후 언급된 컬럼 순서대로 정렬이 적용된다.
-  ORDER BY와 LIMIT
    - LIMIT과 ORDER BY를 같이 쓰는 경우
    - `ORDER BY`와 함께 `LIMIT`을 실행했을 때(출력 행 제한 체크 박스 혹은 `LIMIT`쿼리문 작성), `ORDER BY`가 먼저 실행되게 된다.
    - 즉, 몇 줄의 행으로 출력이 제한되기 전, 이미 정렬된 상태라는 것이다.
