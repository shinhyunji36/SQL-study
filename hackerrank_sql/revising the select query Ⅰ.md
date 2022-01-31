# HackerRank - SQL 문제풀이

## Revising the Select Query Ⅰ
> Query all columns for all American cities in the CITY table with populations larger than 100000. <br>The CountryCode for America is USA.

  ![image](https://user-images.githubusercontent.com/74661937/151806040-d6e68e3c-0b18-4698-8825-6f4729b4cdae.png)

### 풀이
```sql
SELECT *
FROM CITY
WHERE COUNTRYCODE = 'USA' AND POPULATION > 100000
```
