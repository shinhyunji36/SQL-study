# HackerRank - SQL 문제풀이

## Weather Observation Station 7
> Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. <br>Your result cannot contain duplicates.

```sql
SELECT DISTINCT CITY
FROM STATION
WHERE CITY REGEXP('[aeiou]$.*');
```

- 정규표현식 `REGEXP` 사용
   |정규표현식 문자|의미|
   |---|---|
   |[aeiou]|[] 안에 나열된 패턴에 해당하는 문자열을 찾는다.|
   |$|해당 문자열로 끝나는 데이터를 찾는다.|
   |.\*|해당 문자열로 끝난다는 조건을 만족하는 모든 데이터 가져오기.|
   |전체 뜻|aeiou 문자로 끝나는 문자열 데이터 찾기 (ex. Glencoe, Chelsea, Upperco,..)|
