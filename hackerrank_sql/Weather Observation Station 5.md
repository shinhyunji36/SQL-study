# HackerRank - SQL 문제풀이

## Weather Observation Station 5
> Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). <br>If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.
<br>The STATION table is described as follows:

![image](https://user-images.githubusercontent.com/74661937/151828193-327f04f2-7ef5-4c69-83c7-a4c38adc2c9d.png)

- 단일 쿼리가 아니라 나눠진 두 쿼리를 실행해도 된다.
- 만약 동일한 최소 혹은 최대 길이의 문자열을 가진 도시가 여러 곳이라면, 알파벳 순으로 가장 빠른 데이터를 가져온다.

### 풀이
- `LENGTH` : 도시 이름의 문자열 길이를 구하기
- `ORDER BY`와 `LIMIT`을 사용해서 최솟값 / 최댓값 가진 데이터 구하기

  ```sql
  SELECT CITY, 
         LENGTH(CITY)
  FROM STATION
  ORDER BY 2 DESC, 1
  LIMIT 1;

  SELECT CITY,
         LENGTH(CITY)
  FROM STATION
  ORDER BY 2, 1
  LIMIT 1;
  ```
  
  
