# Mode - SQL Basic
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.billboard_top_100_year_end)
```

## SQL Logical Operators - LIKE [🔗](https://mode.com/sql-tutorial/sql-like/)
> **Case** :  컬럼 이름이 `group`인 경우

![image](https://user-images.githubusercontent.com/74661937/148412310-8ff6333a-e98f-4288-815d-402117d74a0f.png)

### 오답 1 
```sql
SELECT *
FROM tutorial.billboard_top_100_year_end
WHERE group LIKE 'Snoop%'
```
- `group` 컬럼을 WHERE절에 그냥 적어준 경우 → 출력 X
- 왜?
  - `group`은 SQL의 함수명이기도 하기 때문에 그냥 적어준다면 컬럼으로 인식하지 못한다.



### 오답 2
```sql
SELECT *
FROM tutorial.billboard_top_100_year_end
WHERE 'group' LIKE 'Snoop%'
```
- `group` 컬럼을 WHERE절에 작은 따옴표를 써서 `'group'`으로 적어준 경우 → 출력 X
- 왜?
  - 컬럼명을 적어줄 땐 큰 따옴표(")를 사용해야한다.
  - 작은 따옴표(')는 컬럼의 non-numerical 값을 지정할 때 사용한다. 
  
### 답
```sql
SELECT *
FROM tutorial.billboard_top_100_year_end
WHERE "group" LIKE 'Snoop%'
```

### **Mode의 설명**
```
The way SQL treats alphabetical ordering is a little bit tricky. 
You may have noticed in the above query that selecting month_name > 'J' will yield only rows in which month_name starts with "j" or later in the alphabet. 
"Wait a minute," you might say. "January is included in the results—shouldn't I have to use month_name >= 'J' to make that happen?" 
SQL considers 'Ja' to be greater than 'J' because it has an extra letter. 
It's worth noting that most dictionaries would list 'Ja' after 'J' as well.
```


## 와일드카드(%, _)

### % 

  ```sql
  SELECT *
  FROM tutorial.billboard_top_100_year_end
  WHERE "group" LIKE 'Snoop%'
  ```
  - `Snoop%` : 대문자 `S` , 소문자 `noop`으로 시작하는 그룹 찾기
  - LIKE는 대소문자를 구별한다.

### _
```sql
SELECT * 
FROM tutorial.billboard_top_100_year_end 
WHERE artist ILIKE 'dr_ke'
```
  - `dr_ke` :  특정 글자를 모를 때 그 자리에 사용한다.


## ILIKE
대소문자를 구별하지 않는 LIKE
```sql
SELECT *
FROM tutorial.billboard_top_100_year_end
WHERE "group" ILIKE 'snoop%'
```
  - 대소문자 상관 없이 그냥 `snoop`으로 시작하는 그룹을 찾는다.


## 예제 오답노트
> Q. Write a query that returns all rows for **which Ludacris was a member of the group**.

- 나의 오답 
  - `Ludacris가 속했던 그룹` (artist 컬럼 중심으로 생각 → Ludacris가 속했던 group 중 하나만 나와야한다고 생각함 / group 컬럼 중심으로 생각하면 팀으로 참여했을 경우 팀 멤버마다 여러번 노래 이름이 중복되어 나오기 때문에 안된다고 이해함.) → 28 rows 출력
```sql
SELECT *
FROM tutorial.billboard_top_100_year_end
WHERE artist ILIKE '%ludacris%'
```

- 답 
  - Ludacris가 속했던 그룹 (group 컬럼 중심 → ludacris가 속했던 그룹이면, 모두 출력) → 61 rows 출력
```sql
SELECT *
FROM tutorial.billboard_top_100_year_end
WHERE "group" ILIKE '%ludacris%'
```
<br>
<br>

> Q. Write a query that returns all rows for which the first artist listed in the group has a name that begins with "DJ".
- 나의 오답
  - 데이터에 대한 이해 하지 않고 쿼리문부터 작성함. `group` 컬럼에 정 그룹으로 활동하는게 아니면 나열식으로 멤버 이름이 적혀있는 것을 파악하지 못했음
  - 따라서 문제에서 제시한 "which the first artist listed in the group" 조건 무시
  - 결과는 오답 : artist 컬럼에서 LIKE 'DJ%'로 찾고 있음 → 5 rows 출력

```sql
SELECT *
FROM tutorial.billboard_top_100_year_end
WHERE artist LIKE 'DJ%'
```
![image](https://user-images.githubusercontent.com/74661937/148416265-56de666b-61cf-4fae-a8ab-bdefe9eb0131.png)


- 답  → 13 rows 출력
```sql
SELECT *
FROM tutorial.billboard_top_100_year_end
WHERE "group" LIKE 'DJ%'
```
![image](https://user-images.githubusercontent.com/74661937/148416446-1c5f2809-6228-4834-9f35-039a7d113b2c.png)

