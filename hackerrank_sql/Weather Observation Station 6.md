# HackerRank - SQL 문제풀이

## Weather Observation Station 6
> Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result cannot contain duplicates. 
 
**Input Format**<br>
The STATION table is described as follows:

![image](https://user-images.githubusercontent.com/74661937/152173138-240bf9e3-b514-493b-9331-cbfb754556e1.png)

### 나의 답
```sql
SELECT DISTINCT CITY
FROM STATION
WHERE LEFT(CITY, 1) IN ('A','E','I','O','U');
```
