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
  SELECT CASE WHEN (A=B AND B=C) THEN "Equilateral"
            WHEN (C >= A+B OR A >= B+C OR B >= C+A ) THEN "Not A Triangle" 
            WHEN (A=B OR B=C OR C=A) THEN "Isosceles"
            ELSE "Scalene"
            END
  FROM TRIANGLES;
  ```
  - 주의점
    - CASE WHEN 조건의 순서를 잘 배치해야한다. <br> 예를 들어 `Isosceles`에 대한 조건을 먼저 쓰거나, ELSE 문에 "Not A Triangle" 조건을 쓰는 경우는 정답과 다른 결과가 나온다.



