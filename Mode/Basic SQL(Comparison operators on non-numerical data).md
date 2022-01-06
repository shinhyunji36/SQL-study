# Mode - SQL Basic
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다.
```

## Comparison operators on non-numerical data [🔗](https://mode.com/sql-tutorial/sql-operators/)
### **Case 1**
```sql
SELECT * 
FROM tutorial.us_housing_units 
WHERE month_name > 'January'
```
- month_name 컬럼에서 `January`, February, April, August, December를 제외한 값이 나온다.
  - 왜? **alphabetical order**로 필터되기 때문!


### **Case 2**
```sql
SELECT * 
FROM tutorial.us_housing_units 
WHERE month_name > 'J'
```
- month_name 컬럼에서 February, April, August, December를 제외한 값이 나온다. (`January`는 출력값에 포함된다.)
- 왜 Case 1과 다르게 `January`가 포함되는가?
  - `Ja`가 **alphabetical order**상 `J`보다 뒤기 때문! (사전에서 단어를 찾는 경우를 생각해보기)

### **Mode의 설명**
```
The way SQL treats alphabetical ordering is a little bit tricky. 
You may have noticed in the above query that selecting month_name > 'J' will yield only rows in which month_name starts with "j" or later in the alphabet. 
"Wait a minute," you might say. "January is included in the results—shouldn't I have to use month_name >= 'J' to make that happen?" 
SQL considers 'Ja' to be greater than 'J' because it has an extra letter. 
It's worth noting that most dictionaries would list 'Ja' after 'J' as well.
```


### **예제 오답노트**
> Write a query that only shows rows for which the month_name starts with the letter "N" or an earlier letter in the alphabet.
  
- 나의 오답
  ```sql
  SELECT *
  FROM tutorial.us_housing_units
  WHERE month_name <= 'N'
  ```
  - 'N'으로 시작되는 모든 글자가 포함되어야 하지만, 나의 오답은 'N'만 포함된다.
  - 따라서 month_name 컬럼에서 'November'를 출력하지 X 
  - month_name 컬럼에서 'N'으로 시작하는 값도 포함해야하는 문제 조건 불충족

- 답
  ```sql
  SELECT *
  FROM tutorial.us_housing_units
  WHERE month_name < 'O'
  ```
  - 알파벳 'O' 이전의 모든 데이터 포함 
  - 따라서 'N'으로 시작되는 'November'도 출력된다.
