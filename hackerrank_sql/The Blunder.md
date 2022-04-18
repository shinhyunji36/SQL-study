# HackerRank - SQL 문제풀이

## The Blunder
- [문제 링크](https://www.hackerrank.com/challenges/the-blunder/problem?isFullScreen=true&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen)
- 0이 눌리지 않는 키보드로 잘못 계산한 값과, 제대로 계산한 값의 차를 구하는 문제
   
### 나의 풀이
- 컨셉
  - `0이 눌리지 않는다` : `REPLACE`로 0을 ''로 대체하기
  - `round it up to the next integer` : `ROUND` 대신 `CEIL` 사용하기 (cf. `FLOOR`)

- 코드 
  ```python
  SELECT CEIL(AVG(Salary) - AVG(REPLACE(Salary, 0, '')))
  FROM EMPLOYEES
  WHERE Salary > 1000 AND Salary < 100000;
  ```
