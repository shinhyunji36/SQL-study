# HackerRank - SQL 문제풀이

## Top Earners
- [문제 링크](https://www.hackerrank.com/challenges/earnings-of-employees/problem?isFullScreen=true&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen)
- `earning = Salary * months`로 정의하고, earning의 최댓값과 명 수를 구하는 문제
   
### 나의 풀이
- 컨셉
  - 집계 함수를 사용하기 위해서는 `GROUP BY`를 함께 써야한다.
  - 처음부터 한 줄의 답안을 출력해내려고 하지 말기
  - `같은 earninig을 가진 명수 구하기` = earning을 기준으로 `GROUP BY` 사용
  - earning을 기준으로 `ORDER BY`/`DESC`를 사용해 내림차순 정렬 후 `LIMIT`을 활용해 맨 첫 결과만 출력하기

- 코드 
  ```python
  SELECT months * salary AS earning, COUNT(*)
  FROM EMPLOYEE
  GROUP BY earning
  ORDER BY 1 DESC
  LIMIT 1;
  ```
  
### 다른 풀이
- `LIMIT`을 사용하지 않는 풀이
  ```python 
  SELECT MAX(months*salary), COUNT(months*salary)
  FROM EMPLOYEE
  WHERE (months*salary) IN (SELECT MAX(months*salary) from EMPLOYEE);
  ```
