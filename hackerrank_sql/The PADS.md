# HackerRank - SQL 문제풀이

## The PADS
> 다음 두 개의 결과 집합을 생성합니다.<br>
> 1. `OCCUPATIONS`에 있는 모든 이름을 알파벳순으로 출력하고, 바로 뒤에 각 직업의 첫 글자를 괄호로 묶는 쿼리를 작성. <br>For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S).<br>
> 
> 2. `OCCUPATIONS`에서 각 직업이 몇번 나왔는지 COUNT하는 쿼리를 작성하고 이를 오름차순으로 정렬 후 아래와 같은 형식으로 출력합니다.<br>
   → ```
    There are a total of [occupation_count] [occupation]s.
    ```
    
    [occupation] : 소문자로 구성된 직업 이름
    [occupation_count] : 먼저 count 값의 오름차순으로 정렬한다. 그 후 직업 count 값이 동일한 경우가 있을 때, [occupation]의 알파벳 오름차순으로 정렬한다.


- `OCCUPAIONS` 테이블의 컬럼별 데이터 타입<br>
  ![image](https://user-images.githubusercontent.com/74661937/152683487-1660c24d-874c-4d7d-9380-70f967b80a1e.png) <br>

- `OCCUPATIOM` 테이블 데이터 예시<br>
  ![image](https://user-images.githubusercontent.com/74661937/152683511-a1cd2837-ffc8-424a-af38-4615baefa605.png) <br>

- 출력 값 예시
  ```
  Ashely(P)
  Christeen(P)
  Jane(A)
  Jenny(D)
  Julia(A)
  Ketty(P)
  Maria(A)
  Meera(S)
  Priya(S)
  Samantha(D)
  There are a total of 2 doctors.
  There are a total of 2 singers.
  There are a total of 3 actors.
  There are a total of 3 professors.
  ```

### 답안
```sql
SELECT CONCAT(Name, '(', LEFT(Occupation, 1), ')')
FROM OCCUPATIONS
ORDER BY Name;

SELECT CONCAT('There are a total of ', COUNT(Occupation), ' ', LOWER(Occupation), 's.') 
FROM OCCUPATIONS
GROUP BY Occupation
ORDER BY COUNT(Occupation), LOWER(Occupation);

```
  - `CONCAT` : `CONCAT`으로 데이터값과 문자열을 이어붙여 문제에서 원하는 포멧으로 데이터를 출력할 수 있다.
  - 출력값이 `NAME(Occupation의 첫 문자)` 출력 부분과 `There are a total of * occupations.` 출력 부분으로 나뉘는데,<br>
    이런 경우에는 두 경우의 출력 쿼리를 작성하고 `;`로 분리해주면 된다. (윗 쿼리 먼저 실행; → 다음 쿼리 실행;)
      

