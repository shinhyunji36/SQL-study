# HackerRank - SQL 문제풀이

## Revising the Select Query Ⅱ
> Query the NAME field for all American cities in the CITY table with populations larger than 120000. <br>The CountryCode for America is USA. <br>The CITY table is described as follows:

  ![image](https://user-images.githubusercontent.com/74661937/151807119-03cc24ef-715d-4a05-a06d-0dbf9e07f695.png)

### 풀이
  ```SQL
  SELECT NAME
  FROM CITY
  WHERE COUNTRYCODE = 'USA' AND POPULATION > 120000
  ```
