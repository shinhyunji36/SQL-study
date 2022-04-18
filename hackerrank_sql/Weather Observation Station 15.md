# HackerRank - SQL 문제풀이

## Weather Observation Station 15
- [문제 링크](https://www.hackerrank.com/challenges/weather-observation-station-15/problem?isFullScreen=true&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen&h_r=next-challenge&h_v=zen)
- 집계 문제
   
### 나의 풀이
- 컨셉
  - MAX값을 활용할 때
    - CASE 1) 정렬로 줄 세워놓고 LIMIT 1 
    - CASE 2) WHERE 문에 SUBQUERY 사용 ✔

- 코드 
  ```python
  SELECT ROUND(LONG_W, 4)
  FROM STATION
  WHERE LAT_N = (SELECT MAX(LAT_N) FROM STATION WHERE LAT_N < 137.2345);
  ```

  
### 다른 풀이
- `LIMIT` 사용
```PYTHON
SELECT ROUND(LONG_W, 4)
FROM STATION
WHERE LAT_N < 137.2345
ORDER BY LAT_N DESC
LIMIT 1
```
