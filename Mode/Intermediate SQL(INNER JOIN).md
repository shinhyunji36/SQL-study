# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (benn.college_football_teams, benn.college_football_players)
```
## INNER JOIN [🔗](https://mode.com/sql-tutorial/sql-inner-join/)
- `JOIN`의 기본 방식은 `INNER JOIN`이다.
  - 아래 SQL문은 같은 결과를 출력한다.
    ```SQL
    JOIN benn.college_football_teams teams
    ```
    ```SQL
    INNER JOIN benn.college_football_teams teams
    ```
  
- 두 개의 테이블을 `INNER JOIN`하는 경우
  - 두 테이블에서 `JOIN`의 기준인 `ON` statement를 충족하지 못하는 행은 제거되어 출력된다.
  - 즉, `INNER JOIN`은 두 테이블의 교집합에 속한 행만 출력한다.
    ![image](https://user-images.githubusercontent.com/74661937/149262370-682ba588-8684-4329-8999-26f0eb4fa460.png)

### 문제
> Write a query that displays player names, school names and conferences for schools in the "FBS (Division I-A Teams)" division.

### 답
```sql
SELECT players.player_name,
       players.school_name,
       teams.conference
FROM benn.college_football_players players
JOIN benn.college_football_teams teams
  ON players.school_name = teams.school_name
WHERE teams.division = 'FBS (Division I-A Teams)'
```
### 설명
- `FROM`, `JOIN` : 가져오는 테이블의 이름이 너무 길 땐, 한 칸 건너뛰고 테이블의 별칭을 써주면 된다.
- `SELECT players.player_name ~` :  테이블에 지정한 별칭을 참조하여 해당 테이블에서 가져오려는 컬럼을 `SELECT`문에 써준다.
- `ON` : `JOIN`의 기준을 지정한다. 어떤걸 중심으로 `INNER JOIN` 할 것인지
- `WHERE` :  조건을 지정할 때도, 테이블의 별칭을 이용하여 컬럼을 불러온다.



