# HackerRank - SQL 문제풀이

## Type of Triangle
> Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. <br>Output one of the following statements for each record in the table:

- **Equilateral**: It's a triangle with  sides of equal length.
- **Isosceles**: It's a triangle with  sides of equal length.
- **Scalene**: It's a triangle with  sides of differing lengths.
- **Not A Triangle**: The given values of A, B, and C don't form a triangle.

  **Input Format**

    The TRIANGLES table is described as follows:<br>
    ![image](https://user-images.githubusercontent.com/74661937/152646040-35202e2c-9337-4ffb-af0e-89e4e0f17cc0.png)

    Each row in the table denotes the lengths of each of a triangle's three sides.


  **Sample Input**
  
    ![image](https://user-images.githubusercontent.com/74661937/152646068-e2e0c4cc-9122-4183-a961-05399507cf85.png)

  **Sample Output**
    ```
    Isosceles
    Equilateral
    Scalene
    Not A Triangle
    ```
  
  **Explanation**

  - Values in the tuple `(20,20,30)` form an Isosceles triangle, because `A=B`.
  - Values in the tuple `(20,20,20)` form an Equilateral triangle, because `A=B=C`. 
  - Values in the tuple `(20,21,22)` form a Scalene triangle, because `A!=B!=C.
  - Values in the tuple `(13,14,30)` cannot form a triangle because the combined value of sides `A` and `B` is not larger than that of side `C`.




  ### 풀이
  - `CASE WHEN`을 활용해서 삼각형의 세 변에 따라 적절한 삼각형 종류를 할당하기
  - `AND`와 `OR`을 조합해서 `CASE` 조건 할당하기

  ```SQL
  SELECT CASE WHEN A = B AND B = C THEN "Equilateral"
              WHEN C >= A+B OR A >= B+C OR B >= C+A THEN "Not A Triangle" 
              WHEN A = B OR B = C OR C = A THEN "Isosceles"
              ELSE "Scalene"
              END
  FROM TRIANGLES;
  ```
  ```SQL
  SELECT CASE WHEN A = B AND B = C THEN "Equilateral"
              WHEN C >= A+B OR A >= B+C OR B >= C+A THEN "Not A Triangle" 
              WHEN A!=B AND B!=C AND C!=A THEN "Scalene"
              ELSE "Isosceles"
              END
  FROM TRIANGLES;
  ```
  
  
  - **주의점**
    - **WHEN 절 조건문의 순서를 잘 배치해야한다.** <br> 예를 들어 `Isosceles`에 대한 조건을 맨 처음 쓰거나, ELSE 문(맨 마지막 조건)에 `Not A Triangle` 조건을 쓰는 경우는 정답과 다른 결과가 나온다.
    - `WHEN` 절에서 첫번째 조건을 충족시킨 데이터는 그 다음 WHEN 절까지 내려오지 않는다.(포함되지 않는다.)
    - 즉, 윗 조건을 충족시키지 못한 데이터가 그 다음 조건절로 내려오는 것. 
    - **이 문제에서는 맨 처음 조건(`Equilateral`)과 두번째 조건(`Not A Triangle`)의 순서를 맞추는 것이 중요하다.**

### 풀이 2 
- 항상 `C`컬럼이 삼각형에서 가장 긴 변인 경우

  ```sql
  SELECT CASE WHEN A=B AND B=C THEN "Equilateral"
              WHEN C >= A+B THEN "Not A Triangle" 
              WHEN A!=B AND B!=C AND C!=A THEN "Scalene"
              ELSE "Isosceles"
              END
  FROM TRIANGLES;
  ```
  
  ```SQL
   SELECT CASE WHEN A=B AND B=C THEN "Equilateral"
              WHEN C >= A+B THEN "Not A Triangle" 
              WHEN A = B OR B = C OR C = A THEN "Isosceles"
              ELSE "Scalene"
              END
  FROM TRIANGLES;
  ```
  
  - 이 경우에도 역시 `WHEN`절의 순서가 중요.









