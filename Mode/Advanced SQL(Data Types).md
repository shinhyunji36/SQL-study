# Mode - SQL Advanced
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.crunchbase_companies_clean_date)
```
## Data Types [🔗](https://mode.com/sql-tutorial/sql-data-types/)
|Imported as|Stored as|With these rules|
|---|---|---|
|String|VARCHAR(1024)|최대 필드 길이가 1024자인 모든 문자|
|Date/Time|TIMESTAMP|년, 월, 일, 시, 분 및 초 값을 YYYY-MM-DD hh:mm:ss로 저장한다.|
|Number|DOUBLE PRECISION|숫자, 최대 17개의 유효 자릿수 소수 자릿수|
|Boolean|BOOLEAN|True OR False|


## 컬럼 데이터 타입 변경
- `CAST` OR `CONVERT`
- 예시
  - `CAST(column_name AS 데이터 타입)`
  - `column_name :: 데이터 타입`

## 예시 문제
> funding_total_usd테이블의 및 founded_at_clean열 tutorial.crunchbase_companies_clean_date을 <br>각각 다른 형식화 함수를 사용하여 문자열(varchar 형식)로 변환
```sql
SELECT CAST(funding_total_usd AS varchar),
       founded_at_clean :: varchar
FROM tutorial.crunchbase_companies_clean_date
```

![image](https://user-images.githubusercontent.com/74661937/150798823-d7ddb47f-6197-49dc-a7f0-b1ef65974c22.png)
