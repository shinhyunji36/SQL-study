# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (Crunchbase data)
```
## Joins Using WHERE or ON [🔗](https://mode.com/sql-tutorial/sql-joins-where-vs-on/)
### case 1 : default
```sql
SELECT companies.permalink AS companies_permalink,
       companies.name AS companies_name, 
       acquisitions.company_permalink AS acquisitions_permalink,
       acquisitions.acquired_at AS acquired_date
FROM tutorial.crunchbase_companies companies
LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
ON companies.permalink = acquisitions.company_permalink
ORDER BY 1
```
![image](https://user-images.githubusercontent.com/74661937/149780900-d1207d50-b00b-4cc9-ba80-a892e9143ef7.png)


### case 2 : `ON` & `AND`
- `ON` & `AND` : 두 테이블이 **JOIN 되기 전에** acquisitions **테이블 필터링**
```sql
SELECT companies.permalink AS companies_permalink,
       companies.name AS companies_name, 
       acquisitions.company_permalink AS acquisitions_permalink,
       acquisitions.acquired_at AS acquired_date
FROM tutorial.crunchbase_companies companies
LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
ON companies.permalink = acquisitions.company_permalink
AND acquisitions.company_permalink != '/company/1000memories'
ORDER BY 1
```
![image](https://user-images.githubusercontent.com/74661937/149781127-6f05e318-9fc5-4628-8ead-aef679449b0d.png)
  

### case 3 : `WHERE`
- `WHERE`절은 두 테이블이 JOIN된 이후, 필터링을 한다.
  - 따라서 acquisitions의 company_permalink 컬럼에서 /company/1000memories 값이 아예 출력되지 않은 것을 알 수 있다.
- `WHERE`절은 NULL 값도 필터링이 가능하기 때문에, 확실히 NULL을 포함하도록 `OR acquisitions.company_permalink IS NULL`이라는 절을 덧붙임
```sql
SELECT companies.permalink AS companies_permalink,
       companies.name AS companies_name,
       acquisitions.company_permalink AS acquisitions_permalink,
       acquisitions.acquired_at AS acquired_date
FROM tutorial.crunchbase_companies companies
LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
ON companies.permalink = acquisitions.company_permalink
WHERE acquisitions.company_permalink != '/company/1000memories'
OR acquisitions.company_permalink IS NULL
ORDER BY 1
```
![image](https://user-images.githubusercontent.com/74661937/149785235-192507c3-ef05-4402-85c6-dd5fd34229de.png)


