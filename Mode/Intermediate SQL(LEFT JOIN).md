# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.crunchbase_companies)
```
## Outer Joins [🔗](https://mode.com/sql-tutorial/sql-outer-joins/)
- Outer Joins
  - [Visual JOIN](https://joins.spathon.com/) - 시각화를 통해 Join 확인해보기
 
      ![image](https://user-images.githubusercontent.com/74661937/149625123-79f033e1-6af5-4298-80c1-8388a54f3afe.png)
  - `LEFT JOIN`   
  - `RIGHT JOIN`  
  - `FULL OUTER JOIN` 


## LEFT JOIN [🔗](https://mode.com/sql-tutorial/sql-left-join/)
![image](https://user-images.githubusercontent.com/74661937/149625381-e1dcd95c-99ed-40ea-9ae4-fbcabb83c106.png)

### 문제 1
> Write a query that performs an inner join between the tutorial. <br>crunchbase_acquisitions table and the tutorial.crunchbase_companies table, <br>but instead of listing individual rows, count the number of non-null rows in each table.

```SQL
SELECT COUNT(acquistions.company_permalink) AS acquisitions_count,
       COUNT(companies.permalink) AS company_count
FROM tutorial.crunchbase_acquisitions acquisitions
JOIN tutorial.crunchbase_companies companies
ON acquisitions.company_permalink = companies.permalink
```
![image](https://user-images.githubusercontent.com/74661937/149626175-b6386d27-6a54-40e6-afc7-ba2970b78889.png)

- `COUNT`는 기본적으로 `non-null` rows만 카운트 한다.


### 문제 2
> Modify the query above to be a LEFT JOIN. Note the difference in results.
```SQL
SELECT COUNT(companies.permalink) AS company_count,
       COUNT(acquisitions.company_permalink) AS acquisition_count
FROM tutorial.crunchbase_companies companies
LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
ON companies.permalink = acquisitions.company_permalink
```
![image](https://user-images.githubusercontent.com/74661937/149626260-5fa9b26a-d1dd-4c97-a906-7dc384a63ff2.png)

