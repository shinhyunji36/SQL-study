# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (benn.college_football_players)
```

## CASE [🔗](https://mode.com/sql-tutorial/sql-case/)
- if/then 로직을 조정할 때 사용한다.
- 최소한 한 쌍의 **`WHEN`, `THEN`과 함께 사용**한다. (Excel에서 IF/THEN과 같은 역할)
- `CASE WHEN`은 붙여 쓴다.
- 모든 `CASE`문은 **`END`문으로 끝나야 한다.**
- `ELSE`문은 선택사항. `WHEN`/`THEN`문으로 정의되지 않은 그 외의 값을 처리하는 방법을 명시할 수 있다.

  ```SQL
  SELECT player_name,
         year,
         CASE WHEN year = 'SR' THEN 'yes'
              ELSE NULL END AS is_a_senior
  FROM benn.college_football_players
  ```
  ![image](https://user-images.githubusercontent.com/74661937/149052996-98674cb3-615b-4e3d-b180-039ca1864f84.png)

  ```
  1. CASE WHEN year = 'SR': 만약 year 컬럼에 'SR' 이라는 값을 가진 행이 있다면 (조건문), True
  2. THEN 'yes' : 주어진 행에 대하여 해당 조건문이 True라면, 'yes' 값 부여 (새로 만든 is_a_senior 컬럼에!)
  3. ELSE NULL END : 조건문이 False인 행은 NULL 값을 부여 (is_a_senior 컬럼에)
  4. 이 모든 조건을 충족시키면서, player_name과 year 컬럼의 모든 값을 검색하고 표시한다.
  ```

- 위의 쿼리문에서 NULL 대신 'no'라는 값을 넣고 싶다면, `ELSE`문을 고치면 된다.
    ```SQL
  SELECT player_name,
         year,
         CASE WHEN year = 'SR' THEN 'yes'
              ELSE 'no' END AS is_a_senior
  FROM benn.college_football_players
  ```
  ![image](https://user-images.githubusercontent.com/74661937/149053585-82a9d29e-dd43-4cbe-a5ce-f378f22a26cd.png)

### 문제
>Write a query that includes a column that is flagged "yes" when a player is from California, <br> and sort the results with those players first.

```sql
SELECT player_name,
       state,
       CASE WHEN state = 'CA' THEN 'yes'
       ELSE NULL END AS from_california
FROM benn.college_football_players
ORDER BY from_california
```
![image](https://user-images.githubusercontent.com/74661937/149054219-1da02e27-ff52-4db7-a9dc-b7147040cbf4.png)


### CASE (여러 조건문 사용)
```sql
SELECT player_name,
       weight,
       CASE WHEN weight > 250 THEN 'over 250'
            WHEN weight > 200 THEN '201-250'
            WHEN weight > 175 THEN '176-200'
            ELSE '175 or under' END AS weight_group
FROM benn.college_football_players
```
![image](https://user-images.githubusercontent.com/74661937/149054728-2f95fa61-a38b-495b-8f75-6be9cc5e7678.png)

- 주의 사항
  - 겹치지 않는 `WHEN`/`THEN`문을 만드는 것이 중요하다.
  - 위의 쿼리문은 250 이상의 값에서 모든 `WHEN`/`THEN`문이 중첩된다.
  - 중첩되지 않기 위해 다시 쿼리문을 작성해보면,
    ```SQL
    SELECT player_name,
           weight,
           CASE WHEN weight > 250 THEN 'over 250'
                WHEN weight > 200 AND weight <= 250 THEN '201-250'
                WHEN weight > 175 AND weight <= 200 THEN '176-200'
                ELSE '175 or under' END AS weight_group
    FROM benn.college_football_players
    ```
    ![image](https://user-images.githubusercontent.com/74661937/149055552-8bccfd26-b9a2-4ab4-95b7-7cd6dff63f9b.png)

### 문제
> Write a query that selects all columns from benn.college_football_players <br> and adds an additional column that displays the player's name if that player is a junior or senior.

### 부등호 사용
```sql
SELECT *,
      CASE WHEN year = 'JR' OR year = 'SR' THEN player_name
      ELSE NULL END AS junior_or_senior
FROM benn.college_football_players
```
![image](https://user-images.githubusercontent.com/74661937/149068879-5bf707b2-54d8-461c-880c-568f9eb1bf0a.png)


### IN 사용한 Mode 답
```sql
SELECT *,
      CASE WHEN year IN ('JR', 'SR') THEN player_name
      ELSE NULL END AS upperclass_player_name
FROM benn.college_football_players
```
![image](https://user-images.githubusercontent.com/74661937/149068934-8127fc41-4e63-4be7-ae02-e3e2d953f81b.png)


### 집계 함수와 CASE
```SQL
SELECT CASE WHEN year = 'FR' THEN 'FR'
            ELSE 'Not FR' END AS year_group,
            COUNT('year_group') AS count --COUNT(1)으로 써도 된다.
FROM benn.college_football_players
GROUP BY CASE WHEN year = 'FR' THEN 'FR'
              ELSE 'Not FR' END
```
![image](https://user-images.githubusercontent.com/74661937/149071884-3013b66e-f27d-4edc-8a87-d3af649bc318.png)


**설명**
```
1. SELECT CASE WHEN year = 'FR' THEN 'FR' ELSE 'Not FR' END AS year_group
  : year 컬럼에서 'FR' 값을 가지고 있는 행은 'FR'을, 아닌 행은 'Not FR' 값을 할당합니다. (새로 만든 year_group 컬럼에)
2. GROUP BY CASE WHEN year = 'FR' THEN 'FR' ELSE 'Not FR' END
  : GROUP BY를 통해 'FR'과 'Not FR'을 가진 집단을 분류합니다.
3. COUNT('year_group') AS count
  : 분류된 값을 COUNT를 활용하여 집계합니다.
```

- 질문
  > 'FR'만 COUNT 하는 것이라면, `WHERE`을 사용하여 그 값을 COUNT 하면 되는 거 아닌가?
   ```SQL
   SELECT COUNT(1) AS fr_count
   FROM benn.college_football_players
   WHERE year = 'FR'
   ```
   ![image](https://user-images.githubusercontent.com/74661937/149071847-2e789708-03cf-4f3e-8b34-5845dedb90f0.png)
  - 그러나 `WHERE`는 하나의 조건만 COUNT할 수 있다.
  - 따라서 여러 조건을 COUNT 하기 위해서는, `CASE WHEN` ~ `THEN` `END` 구문을 사용해야 한다.
  
 ```sql
 SELECT CASE WHEN year = 'FR' THEN 'FR'
             WHEN year = 'SO' THEN 'SO'
             WHEN year = 'JR' THEN 'JR'
             WHEN year = 'SR' THEN 'SR'
             ELSE 'No Year Data' END AS year_group,
             COUNT(1) AS count
 FROM benn.college_football_players
 GROUP BY 1 --GROUP BY year_group
 ```
 ![image](https://user-images.githubusercontent.com/74661937/149074978-b6c91245-b9c8-436f-bfc8-3ef92e6069ff.png)


- `CASE` 조건문을 `GROUP BY`에 다시 복사/붙여넣기 할 때, `AS`구문은 빼고 적는다.
   ```sql
   SELECT CASE WHEN year = 'FR' THEN 'FR'
               WHEN year = 'SO' THEN 'SO'
               WHEN year = 'JR' THEN 'JR'
               WHEN year = 'SR' THEN 'SR'
               ELSE 'No Year Data' END AS year_group,
               COUNT(1) AS count
   FROM benn.college_football_players
   GROUP BY CASE WHEN year = 'FR' THEN 'FR'
                WHEN year = 'SO' THEN 'SO'
                WHEN year = 'JR' THEN 'JR'
                WHEN year = 'SR' THEN 'SR'
                ELSE 'No Year Data' END
   ```

### 문제
> Write a query that calculates the combined weight of all underclass players (FR/SO) in California <br> as well as the combined weight of all upperclass players (JR/SR) in California.

### 오답
```sql
SELECT CASE WHEN year IN ('FR','SO') THEN 'underclass_players'
            WHEN year IN ('JR', 'SR') THEN 'upperclass_players'
            ELSE NULL END AS player_class
            SUM(weight)
FROM benn.college_football_players
WHERE state = 'CA'
GROUP BY player_class
```
- `ELSE` ~ `END`로 끝나는 `CASE`구문 이후에 `,`를 안찍었음.
- `SELECT` `SUM(weight)`으로 붙여 써야하지만, 들여쓰기를 잘못함.
- 오류 출력
  ![image](https://user-images.githubusercontent.com/74661937/149081365-6790e518-0403-401e-8a3a-320075624e95.png)


### 답
```sql
SELECT CASE WHEN year IN ('FR','SO') THEN 'underclass_players'
            WHEN year IN ('JR', 'SR') THEN 'upperclass_players'
            ELSE NULL END AS player_class,
       SUM(weight) AS combined_player_weight
FROM benn.college_football_players
WHERE state = 'CA'
GROUP BY player_class
```
 ![image](https://user-images.githubusercontent.com/74661937/149081292-2d4b2456-d87f-4645-aea8-aa424d35b32b.png)



### 집계 함수 내에서 CASE 사용하기 (수직 → 수평)
- 일반적인 display (수직)
  ```sql
  SELECT CASE WHEN year = 'FR' THEN 'FR'
              WHEN year = 'SO' THEN 'SO'
              WHEN year = 'JR' THEN 'JR'
              WHEN year = 'SR' THEN 'SR'
              ELSE 'No Year Data' END AS year_group,
              COUNT(year_group) AS count --COUNT(1) AS count
  FROM benn.college_football_players
  GROUP BY 1 --GROUP BY year_group
  ```
  ![image](https://user-images.githubusercontent.com/74661937/149082831-e6743804-30e4-4b14-8804-3203b29e865a.png)

- 수평적 display
  ```sql
  SELECT COUNT(CASE WHEN year = 'FR' THEN 1 ELSE NULL END) AS fr_count,
       COUNT(CASE WHEN year = 'SO' THEN 1 ELSE NULL END) AS so_count,
       COUNT(CASE WHEN year = 'JR' THEN 1 ELSE NULL END) AS jr_count,
       COUNT(CASE WHEN year = 'SR' THEN 1 ELSE NULL END) AS sr_count
  FROM benn.college_football_players
  ```
  ![image](https://user-images.githubusercontent.com/74661937/149083050-2c6f7811-7275-40d1-a8a1-80d913904168.png)


### 문제
> Write a query that displays the number of players in each state, with FR, SO, JR, and SR players in separate columns and another column for the total number of players. 
> <br> Order results such that states with the most players come first.
- `SELECT` 해야하는 컬럼들
  ```
  state, 
  FR, SO, JR, SR players in seperate columns,
  total number of players
  ```
- `displays the number of players in each state`
   : `GROUP BY` state

- `order results such that states with the most players come first.`
  : state 카테고리 별로 그룹 집계된 값 기준으로 `ORDER BY` (내림차순)

### 답
```sql
SELECT state,
       COUNT(CASE WHEN year = 'FR' THEN 1 ELSE NULL END) AS fr_count,
       COUNT(CASE WHEN year = 'SO' THEN 1 ELSE NULL END) AS so_count,
       COUNT(CASE WHEN year = 'JR' THEN 1 ELSE NULL END) AS jr_count,
       COUNT(CASE WHEN year = 'SR' THEN 1 ELSE NULL END) AS sr_count,
       COUNT(state) AS total_player
FROM benn.college_football_players
GROUP BY state
ORDER BY total_player DESC
```


### 문제
> Write a query that shows the number of players at schools with names that start with A through M, <br> and the number at schools with names starting with N - Z.


### 오답
```sql
SELECT CASE WHEN school_name BETWEEN 'A' AND 'M' THEN 1
            WHEN school_name BETWEEN 'N' AND 'Z' THEN 2 END AS school,
       COUNT(1) AS count
FROM benn.college_football_players
GROUP BY school
```
![image](https://user-images.githubusercontent.com/74661937/149133076-555fef73-cd4b-49d5-8297-eca1691f4066.png)


### 답
```sql
SELECT CASE WHEN school_name < 'n' THEN 'A-M'
            WHEN school_name >= 'n' THEN 'N-Z'
            ELSE NULL END AS school_name_group,
       COUNT(1) AS players --COUNT('school_name_group') AS players
FROM benn.college_football_players
GROUP BY 1 --GROUP BY 
```
![image](https://user-images.githubusercontent.com/74661937/149133121-95dbbbf0-8186-4075-8674-023b52ec72b7.png)

