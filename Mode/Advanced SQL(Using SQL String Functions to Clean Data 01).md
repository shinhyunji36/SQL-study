# Mode - SQL Advanced
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다.(tutorial.sf_crime_incidents_2014_01 : 2013년 11월 1일부터 2014년 1월 31일까지의 3개월 동안의 샌프란시스코 범죄 사건에 대한 데이터)
```
## Using SQL String Functions to Clean Data 01 [🔗](https://mode.com/sql-tutorial/sql-string-functions-for-cleaning/)
### LEFT, RIGHT, LENGTH
  - `LEFT(데이터, 글자수)` : 데이터의 왼쪽에서 글자 수만큼 짤라서 보여줌
  - `RIGHT(데이터, 글자수)`
  - `LENGTH(데이터)` : 데이터의 글자 수를 반환한다.
  
```SQL
SELECT incidnt_num,
       date,
       LEFT(date, 10) AS cleaned_date,
       RIGHT(date, LENGTH(date) - 11) AS cleaned_time
FROM tutorial.sf_crime_incidents_2014_01
```
![image](https://user-images.githubusercontent.com/74661937/151177825-7394ad5d-c4c0-42b8-a1ea-d5f8412050de.png)

### TRIM
  > `TRIM(leading/both/trailing, '제거할 문자', FROM 컬럼명)`
  - 맨 앞(leading)/ 양 옆(both)/ 맨 뒤(trailing)에서 특정 문자를 제거한다.
  ```sql
  SELECT location,
       TRIM(leading '()' FROM location) AS leading_trim
  FROM tutorial.sf_crime_incidents_2014_01
  ```
  ![image](https://user-images.githubusercontent.com/74661937/151179170-c9fe1b7e-0204-42fb-a3cc-4a7c26616f84.png)



### POSITION and STRPOS
- `POSITION` 
> `POSITION('특정 문자' IN 컬럼명)`
  - 대상 문자열에서 특정 문자가 처음 나타나는 순서를 반환 (문자의 맨 처음부터 시작)
  - 대소문자를 구별하기 때문에 `UPPER`또는 `LOWER` 함수와 함께 사용할 수 있다.

 ```SQL
 SELECT incidnt_num,
       descript,
       POSITION('A' IN descript) AS a_position
 FROM tutorial.sf_crime_incidents_2014_01
 ```
 ![image](https://user-images.githubusercontent.com/74661937/151180359-057c6cd0-0b69-4321-a47a-038b5a055a06.png)


- `STRPOS`
> `STRPOS(컬럼명, '특정 문자')
  -  `POSITION`과 동일한 결과 반환 (문자의 맨 처음부터 시작)
  - 대소문자를 구별하기 때문에 `UPPER`또는 `LOWER` 함수와 함께 사용할 수 있다.
  
```SQL
SELECT incidnt_num,
       descript,
       STRPOS(descript, 'A') AS a_position
FROM tutorial.sf_crime_incidents_2014_01
```

### SUBSTR
- `SUBSTR`은 `POSITION`, `STRPOS`와 동일하지만 시작점을 정할 수 있다.
> `SUBSTR(문자(컬럼명), 시작 문자 위치, 특정문자`)

```SQL
SELECT incidnt_num,
       date,
       SUBSTR(date, 4, 2) AS day
 FROM tutorial.sf_crime_incidents_2014_01
 ```
 ![image](https://user-images.githubusercontent.com/74661937/151182591-822d648e-60ad-471a-ba26-b303e57bf044.png)
  - `date` 컬럼 데이터의 4번째, 5번째 문자 반환


### CONCAT
- 여러 컬럼의 문자열을 결합할 때 사용
  ```SQL
  SELECT incidnt_num,
         day_of_week,
         LEFT(date, 10) AS cleaned_date,
         CONCAT(day_of_week, ', ', LEFT(date, 10)) AS day_and_date
  FROM tutorial.sf_crime_incidents_2014_01
  ```
- `CONCAT` 대신 `||`를 사용해서 같은 결과를 낼 수 있다.
  ```SQL
  SELECT incidnt_num,
       day_of_week,
       LEFT(date, 10) AS cleaned_date,
       day_of_week || ', ' || LEFT(date, 10) AS day_and_date
  FROM tutorial.sf_crime_incidents_2014_01
  ```
  ![image](https://user-images.githubusercontent.com/74661937/151183481-3c50973c-9671-48e1-8de5-729cac6bc988.png)

### UPPER & LOWER
```SQL
SELECT incidnt_num,
       address,
       UPPER(address) AS address_upper,
       LOWER(address) AS address_lower
  FROM tutorial.sf_crime_incidents_2014_01
```
 ![image](https://user-images.githubusercontent.com/74661937/151183927-872110df-4f38-4d61-989d-945a6e91fc9f.png)

- 문제
  > `category` 필드를 반환하지만 첫 글자는 대문자로, 나머지 글자는 소문자로 반환하는 쿼리를 작성하세요.
  ```SQL
  SELECT incidnt_num,
         category,
         CONCAT(UPPER(LEFT(category, 1)), LOWER(RIGHT(category, LENGTH(category) - 1))) AS category_cleaned 
  FROM tutorial.sf_crime_incidents_2014_01
  ```
  ![image](https://user-images.githubusercontent.com/74661937/151184624-ec08d038-9524-4fad-88cb-f0944e67420d.png)
  
 ### COALESCE
 - null 값을 대체하는데 사용 
 - 해당 컬럼에 null 값일 때는 `COALESCE`로 인해 만들어진 새로운 컬럼에
  - 지정한 문자열이 쓰여진다.
- 반대로 해당 컬럼에 null 값이 아닐 때는, 해당 컬럼의 값이 `COALESCE`로 인해 만들어진 새로운 컬럼에 쓰여진다.
 
 ```sql
 SELECT incidnt_num,
       descript,
       COALESCE(descript, 'No Description')
  FROM tutorial.sf_crime_incidents_cleandate
 ORDER BY descript DESC
 ```
![image](https://user-images.githubusercontent.com/74661937/151185548-4f2ae18a-7c9b-4486-9249-724fbc734f49.png)


