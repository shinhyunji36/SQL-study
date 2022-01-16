# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (tutorial.crunchbase_companies)
```
## RIGHT JOIN [🔗](https://mode.com/sql-tutorial/sql-right-join/)
![image](https://user-images.githubusercontent.com/74661937/149662307-c84c445d-65b9-49b4-9d98-4f7ed2b5164f.png)
- **`RIGHT JOIN`절의 테이블에 있는 모든 행들**과 해당 테이블과 매치되는 `FROM`절의 테이블을 출력한다.
- `RIGHT JOIN`은 자주 사용되지 않는다.
  - `LEFT JOIN`에서 두 테이블 이름을 바꿔 써주는 것만으로 똑같은 결과가 나오기 때문.
  - 따라서 아래 두 SQL문은 같은 결과를 출력한다.
    ```sql
    SELECT companies.permalink AS companies_permalink,
           companies.name AS companies_name,
           acquisitions.company_permalink AS acquisition_permalink,
           acquisitions.acquired_as AS acquired_date
    FROM tutorial.crunchbase_companies companies
    LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
    ON companies.permalink = acquisitions.company_permalink
    ```
    ```sql
    SELECT companies.permalink AS companies_permalink,
           companies.name AS companies_name,
           acquisitions.company_permalink AS acquisition_permalink,
           acquisitions.acquired_at AS acquired_date
    FROM tutorial.crunchbase_acquisitions acquisitions
    RIGHT JOIN tutorial.crunchbase_companies companies
    ON companies.permalink = acquisitions.company_permalink
    ```
