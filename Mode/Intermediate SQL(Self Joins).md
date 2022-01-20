# Mode - SQL Intermediate
```
Mode의 자체 SQL을 활용하여 실습했습니다.
Mode의 DB에 있는 튜토리얼 데이터를 활용했습니다. (Crunchbase data)
```
## Self Joins [🔗](https://mode.com/sql-tutorial/sql-self-joins/)
- 같은 Table이지만, 다른 별칭을 사용하여 `Self Join`이 가능하다.

### 자기 참조 join의 예제
> `Japan`에서 투자 받은 후, `Great Britain`으로부터 투자를 받은 회사들을 확인해라
  ```SQL
  SELECT DISTINCT japan_investments.company_name,
         japan_investments.company_permalink
  FROM tutorial.crunchbase_investments_part1 japan_investments
  JOIN tutorial.crunchbase_investments_part1 gb_investments
  ON japan_investments.company_name = gb_investments.company_name
  AND gb_investments.investor_country_code = 'GBR'
  AND gb_investments.funded_at > japan_investments.funded_at
  WHERE japan_investments.investor_country_code = 'JPN'
  ORDER BY 1
  ```
  - tutorial.crunchbase_investments_part1을 각각 `japan_investments`와 `gb_investments`로 별칭을 지정해서 Self Join 을 했다.
  
  ![image](https://user-images.githubusercontent.com/74661937/150300861-44dc33b3-4f7c-4e74-b2db-f91ab28f98e2.png)
