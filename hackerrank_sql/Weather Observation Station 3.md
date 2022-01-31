# HackerRank - SQL 문제풀이

## Weather Observation Station 3
> Query a list of CITY names from STATION for cities that have an even ID number.<br>Print the results in any order, but exclude duplicates from the answer. <br>where LAT_N is the northern latitude and LONG_W is the western longitude.
<br>The STATION table is described as follows:

![image](https://user-images.githubusercontent.com/74661937/151820892-bb5acc0e-f2c6-4813-a70e-982123df97fa.png)


### 풀이 
```SQL
SELECT DISTINCT CITY
FROM STATION
WHERE MOD(ID,2) = 0;
```
- **짝수 id를 가진 데이터를 찾아라** : `MOD`를 사용하여 ID를 2로 나눈 값이 0인 ID를 가진 데이터만 출력하기
