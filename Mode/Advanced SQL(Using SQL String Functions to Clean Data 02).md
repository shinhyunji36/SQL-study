# Mode - SQL Advanced
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다.(tutorial.sf_crime_incidents_2014_01 : 2013년 11월 1일부터 2014년 1월 31일까지의 3개월 동안의 샌프란시스코 범죄 사건에 대한 데이터)
```
## Using SQL String Functions to Clean Data 02 [🔗](https://mode.com/sql-tutorial/sql-string-functions-for-cleaning/)

### 문자열 날짜로 변환하기
- 기존 데이터
  ```sql
  SELECT *
  FROM tutorial.sf_crime_incidents_2014_01
  ```
  ![image](https://user-images.githubusercontent.com/74661937/151356863-08570740-7ddf-4982-bb19-38f21c8b698b.png) 
  - `date` 컬럼의 데이터 형식 예 : `01/31/2014 08:00:00 AM +0000` (MM/DD/YYYY의 형태)
  - `time` 커럼의 데이터 형식 예 : `17:00`

### 문제
> Write a query that creates an accurate timestamp using the `date` and `time` columns in tutorial.sf_crime_incidents_2014_01. <br>Include a field that is exactly 1 week later as well.

```sql
SELECT incidnt_num,
       (SUBSTR(date, 7, 4) || '-' || LEFT(date, 2) || '-' || SUBSTR(date, 4, 2) || ' ' || time || ':00')::timestamp AS timestamp,
       (SUBSTR(date, 7, 4) || '-' || LEFT(date, 2) || '-' || SUBSTR(date, 4, 2) || ' ' || time || ':00')::timestamp + INTERVAL '1 week' AS timestamp_plus_interval
FROM tutorial.sf_crime_incidents_2014_01
``` 
![image](https://user-images.githubusercontent.com/74661937/151356343-dbefb255-04d4-47c6-bbe1-a760a7461ab2.png)
- `SUBSTR(date, 7, 4)` :  `date` 컬럼의 7번째 문자부터 4개의 문자를 짤라서 가져옴 (01/31/2014 08:00:00 AM +0000 에서 2014)
- `LEFT(date, 2)` : `date` 컬럼의 왼쪽부터 2개의 문자를 짤라서 가져옴 (01/31/2014 08:00:00 AM +0000 에서 01)
- `||` : `CONCAT`과 같은 역할. 문자열을 붙여준다.
- `::timestamp` : 문자열(varchar)을 timestamp 데이터 타입으로 변환
- `+ INTERVAL '1 week'` : timestamp 값에 1주(7일)를 더한다.


### Cleaned_date의 경우
- 날짜 데이터가 `YYYY-MM-DD HH:MM:SS` 형태로 Cleaned_Date일 경우
  - `EXTRACT`를 사용하여 년, 월, 일, 시, 분, 초 등을 따로 추출할 수 있다.
   ```sql
   SELECT cleaned_date,
         EXTRACT('year'   FROM cleaned_date) AS year,
         EXTRACT('month'  FROM cleaned_date) AS month,
         EXTRACT('day'    FROM cleaned_date) AS day,
         EXTRACT('hour'   FROM cleaned_date) AS hour,
         EXTRACT('minute' FROM cleaned_date) AS minute,
         EXTRACT('second' FROM cleaned_date) AS second,
         EXTRACT('decade' FROM cleaned_date) AS decade,
         EXTRACT('dow'    FROM cleaned_date) AS day_of_week
    FROM tutorial.sf_crime_incidents_cleandate
   ```
   ![image](https://user-images.githubusercontent.com/74661937/151359836-28ee0561-185a-4b28-9695-67698380b9b8.png)
   
   - `DATE_TRUNC`를 사용해서 가장 가까운 측정단위로 반올림할 수도 있다.
   ```sql
   SELECT cleaned_date,
         DATE_TRUNC('year'   , cleaned_date) AS year,
         DATE_TRUNC('month'  , cleaned_date) AS month,
         DATE_TRUNC('week'   , cleaned_date) AS week,
         DATE_TRUNC('day'    , cleaned_date) AS day,
         DATE_TRUNC('hour'   , cleaned_date) AS hour,
         DATE_TRUNC('minute' , cleaned_date) AS minute,
         DATE_TRUNC('second' , cleaned_date) AS second,
         DATE_TRUNC('decade' , cleaned_date) AS decade
   FROM tutorial.sf_crime_incidents_cleandate
   ```
   ![image](https://user-images.githubusercontent.com/74661937/151360326-1318d5e1-5019-4dcd-9f7e-5e7191d57219.png)


### 오늘 날짜 가져오는 경우
- `CURRENT_DATE`/`CURRENT_TIME`/`CURRENT_TIMESTAMP` 또는 `LOCALTIME`/`LOCALTIMESTAMP`를 활용해서 현지 시간 및 날짜를 가져올 수 있다.
- 현지 시간의 기준은 'UTC'
- `FROM`을 사용하지 않아도 됨!

```sql
SELECT CURRENT_DATE AS date,
       CURRENT_TIME AS time,
       CURRENT_TIMESTAMP AS timestamp,
       LOCALTIME AS localtime,
       LOCALTIMESTAMP AS localtimestamp,
       NOW() AS now
```
![image](https://user-images.githubusercontent.com/74661937/151361648-4befe60c-8d33-472d-b045-ea9e9d154731.png)

- 한국 기준 현재 날짜 및 시간을 불러오려면 아래 쿼리문을 실행하면 된다.
 ```SQL
 SELECT NOW() AT TIME ZONE 'Asia/Seoul'
 ```
![image](https://user-images.githubusercontent.com/74661937/151363161-40b7c3ab-4cba-48a9-a184-82c7ff5eef70.png)
![image](https://user-images.githubusercontent.com/74661937/151363187-cf867e1a-b5e6-4668-a5f4-82750dd82f0f.png)
