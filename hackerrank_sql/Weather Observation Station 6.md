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
 - 그러나 이 경우는 `CITY` 컬럼의 모든 데이터가 대문자로 시작한다는 전제 하에 가능함.<br>(그러나 HackerRank에서는 대문자든 소문자든 동일하게 작동했다)
 - 그래서 다른 사람들의 답을 찾아봤음. 
   1. `regexp`를 사용한 정규표현식 사용
   2. `LIKE`를 사용


### 찾아본 답 1
```SQL
SELECT DISTINCT CITY
FROM STATION
WHERE CITY REGEXP '^[aeiou].*'
```
- `REGEXP` 해석

   |정규표현식 문자|의미|
   |---|---|
   |[aeiou]|특정 문자인 a,e,i,o,u|
   |^|주어진aeiou 중 특정 문자(a 또는 e 또는..)로 **시작**하는 CITY의 데이터 가리킨다.|
   |.\*|시작 문자 이후의 문자는 어떤 것이든 상관 없다.(`LIKE`의 `%`와 같은 의미)|
   |전체 뜻|aeiou 문자로 시작하는 문자열 찾기(ex.A~로 시작하는 데이터 찾기)|
 


### 찾아본 답 2
```sql
SELECT DISTINCT CITY 
FROM STATION 
WHERE CITY LIKE "[aeiou]%"
```


