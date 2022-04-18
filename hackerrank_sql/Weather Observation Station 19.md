# HackerRank - SQL 문제풀이

## Weather Observation Station 19
- [문제 링크](https://www.hackerrank.com/challenges/weather-observation-station-19/problem?isFullScreen=true)
- 두 점의 유클리디안 거리 구하기
   
### 나의 풀이
  - `POW(A, B)` : A의 B 제곱 계산
  - `SQRT` : 루트 씌워 제곱근 구하기

- 코드 
  ```python
  SELECT ROUND(
      SQRT(
          POW((MAX(LAT_N)-MIN(LAT_N)),2) + POW((MAX(LONG_W)-MIN(LONG_W)),2)
      ),4)
  FROM STATION;
  ```
