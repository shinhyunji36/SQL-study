# HackerRank - SQL 문제풀이

## Occupations
> Pivot the `Occupation` column in `OCCUPATIONS` so that **each Name is sorted alphabetically** and **displayed underneath its corresponding Occupation**. <br>The output **column headers should be Doctor, Professor, Singer, and Actor**, respectively.<br>
Note: Print NULL when there are no more names corresponding to an occupation.

**Input Format**

The OCCUPATIONS table is described as follows:<br>
  ![image](https://user-images.githubusercontent.com/74661937/152795088-2c542c15-95f4-481b-a8eb-2aae34cd390c.png)
  
Occupation will only contain one of the following values: Doctor, Professor, Singer or Actor.

**Sample Input**<br>
  ![image](https://user-images.githubusercontent.com/74661937/152795177-4327a8a9-5a7c-4aff-ae74-b78ef0a28c14.png)

**Sample Output**
  ```
  Jenny    Ashley     Meera  Jane
  Samantha Christeen  Priya  Julia
  NULL     Ketty      NULL   Maria
  ```
  |Doctor|Professor|Singer|Actor|
  |---|---|---|---|
  |Jenny|Ashley|Meera|Jane|
  |Smantha|Christeen|Priya|Julia|
  |NULL|Ketty|NULL|Maria|
  
  


**Explanation**<br>
The first column is an alphabetically ordered list of Doctor names.<br>
The second column is an alphabetically ordered list of Professor names.<br>
The third column is an alphabetically ordered list of Singer names.<br>
The fourth column is an alphabetically ordered list of Actor names.<br>
The empty cell data for columns with less than the maximum number of names per occupation (in this case, the Professor and Actor columns) are filled with NULL values.



### 풀이
  ```sql
  SELECT MAX(CASE WHEN(Occupation = 'Doctor') THEN Name END) AS Doctor,
         MAX(CASE WHEN(Occupation = 'Professor') THEN Name END) AS Professor,
         MAX(CASE WHEN(Occupation = 'Singer') THEN Name END) AS Singer,
         MAX(CASE WHEN(Occupation = 'Actor') THEN Name END) AS Actor
  FROM (SELECT *, row_number() over (partition by Occupation order by Name) rn
  FROM OCCUPATIONS) sub
  GROUP BY rn;
  ```
- 서브 쿼리
  ```sql 
  SELECT *, row_number() over (partition by Occupation order by Name)
  FROM OCCUPATIONS
  ```
    ![image](https://user-images.githubusercontent.com/74661937/152795889-efd7a512-5d94-41a6-a491-b129ef04d01d.png)

  - `row_number()` <br>
    : DB내 row의 수를 세기 위한 함수<br>
     ex. `SELECT row_number() over (order by column_name) AS rn` <br>
      : 특정 컬럼을 기준으로 오름차순 정렬된 rows에 순서대로 행 번호를 붙인다. (즉, 행 번호가 기입된 컬럼을 만든다.)
    
  - `row_num() over (partition by column_name ~)` <br>
    : 기준 컬럼에서 동일한 값을 가진 행끼리 파티션을 나눠 구성하고, 파티션 마다 행 번호를 부여한다. <br>
      새로운 파티션의 행 번호는 다시 1부터 시작된다.

- 전체 쿼리
  1. `서브 쿼리` 출력 결과에서 `Occupation`의 카테고리 별로 파티션이 나눠져 행 번호를 부여받았다.<br>
    ex. Doctor 1, Doctor 2, Doctor 3 / Actor 1, Actor 2 ... 
  
  2. `SELECT MAX(CASE WHEN(Occupation = 'Doctor') THEN Name END) AS Doctor` 의 경우
      1. `GROUP BY rn`을 통해 같은 row_number를 기준으로 그룹화 한 후,<br> 
        (ex. Doctor 1, Actor 1 / Doctor 2, Actor 2,.. → **rn(row_number) = 1인 경우, Aamina Doctor 1, Eve Actor 1, ...**)<br>
     
          - 각 행에 각 직업(Occupation)별로 이름이 가장 첫번째로 오는 사람(첫 행) → 두번째로 오는 사람(두번째 행) → ...순으로 데이터를 출력해야 함.
          - 이를 위해 먼저 각 직업 별로 이름이 가장 첫번째 순서인 이름들(row_num = 1; name Docotr 1, name Actor 1, ..), 두번째 순서의 이름들(row_num = 2; name Doctor 2, name Actor2 ..), ...끼리 `GROUP BY`를 실행

      2. `CASE WHEN (Occupation = 'Doctor') THEN Name END`
          - `Occupation` 컬럼의 값이 Doctor일 때, `Name`의 값을 출력한다. (**Aamina Doctor 1, Eve Actor 1, ... → Aamina**)


      3. `MAX`(집계함수)
          - 가장 상위 데이터인 **Aamina Doctor 1**의 `Name`인 **Aamina**를 출력한다.
          - 어차피 `row_num = 1`로 그룹화된 데이터 중 `Occupation = Doctor`인 것은 `Aamina Doctor 1`밖에 없었지만,<br>
            `GROUP BY` 사용을 위해 집계함수 `MAX` 사용했음. 조건을 충족하는 레코드가 하나뿐이기 때문에 집계함수 `MIN`을 사용해도 같은 결과를 출력한다.<br>
            (*`GROUP BY`를 사용하면 결과 출력을 위해 집계 함수를 함께 사용 해야한다.)
  
  - 전체 쿼리 실행 결과
  
    ![image](https://user-images.githubusercontent.com/74661937/152798226-e0a1321c-e3ec-4928-a0e1-45c4339a7701.png)

  

