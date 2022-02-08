# HackerRank - SQL 문제풀이

## Binary Tree Nodes
- BST 테이블
  - 컬럼 `N` : 이진 트리의 노드 값
  - 컬럼 `P` : `N`의 상위(Parent) 노드 값 (상위 노드가 존재하지 않는 경우 `P`의 값은 `NULL`)
  ![image](https://user-images.githubusercontent.com/74661937/153007199-72333f01-f174-45ba-ac87-e78c8603e9e0.png)

> 노드 값(`N`)을 기준으로 정렬된 이진 트리의 노드 타입을 찾는 쿼리를 작성해라.<br>
> 출력될 노드 타입은 `Root`, `Leaf`, `Inner` 세가지이다. 


**Sample Input**<br>
  ![image](https://user-images.githubusercontent.com/74661937/153007710-c6915fc1-2ea7-4339-b703-2d64d33922af.png)


**Sample Output**
  ```
  1 Leaf
  2 Inner
  3 Leaf
  5 Root
  6 Leaf
  8 Inner
  9 Leaf
  ```
  
**Explanation**<br>
  The Binary Tree below illustrates the sample:<br>
  ![image](https://user-images.githubusercontent.com/74661937/153007893-12eaa788-4e33-43a7-b69a-3890eb8d1448.png)
   - **Root node** : 가장 상위 노드로, 부모 노드가 없다. (ex. 5)
   - **Leaf node** : 가장 하위 노드로, 자식 노드가 없다. (ex. 1, 3, 6, 9)
   - **Inner node** : 루트 노드도, 리프 노드도 아닌 중간에 끼인 노드. inner node는 부모 노드와 자식 노드 모두 가지고 있다. (ex. 2, 8)
   
   
### 풀이
- 컨셉 
  - Root node는 `P` 컬럼에 값이 없을 것. 즉 `NULL`일 것!
  - Leaf node는 `P`값(부모 노드 값)에 존재하지 않는 `N`값(노드 값)을 찾으면 된다. <br>왜냐하면 Leaf node는 자식 노드를 갖지 않아 부모 노드가 될 수 없으므로 <br>부모 노드 값이 모여있는 `P`컬럼에 답이 없을 것이다.
  - Innrt node는 Root와 Leaf를 찾은 나머지 노드들일 것이다.(`ELSE`)

- 코드
  ```sql
  SELECT N,
         CASE WHEN P IS NULL THEN 'Root'
              WHEN N NOT IN (SELECT DISTINCT P FROM BST WHERE P IS NOT NULL) THEN 'Leaf' 
         ELSE 'Inner'  END
  FROM BST
  ORDER BY N;
  ```
- 주의할 점
  1. `CASE WHEN` 조건 여러번 쓸 때
      - 두번째 조건부터는 `WHEN`만 쓰면 된다.
      - `CASE` 조건문에는 `,`를 쓰지 않는다.
      - `END`로 해당 조건문이 끝났음을 꼭 명시해줘야한다.
        ```sql
        CASE WHEN 조건1 THEN '지정값' 
             WHEN 조건 2 THEN '지정값2'
        ELSE '지정값' 
        END
        ```
      
  2. 서브쿼리 작성
      - Leaf를 지정할 때, 컬럼 `P`에 없는 `N`값을 구하고 싶어서 아래처럼 쿼리문을 작성했더랬다. (실행 안됨)
         ```SQL
         CASE WHEN N NOT IN P THEN 'Leaf'
         ```

      - 하지만 `P`에 있는 값들을 가져오기 위해서는 서브쿼리를 사용해 `SELECT`로 값들을 가져와야 한다.
      - `WHERE P IS NOT NULL` : 또한 `P`값이 `NULL`이라면, Leaf가 아니라 Root이므로<br> `WHERE`조건문으로 이 경우도 제외하고 `P` 컬럼의 값들을 가져와야 한다.
      - `SELECT DISTINCT P` : 중복되는 `P`컬럼의 값을 제외하고 유일값만 가져온다. 부모노드는 가지고 있는 자식 수만큼(이진 트리니까 2번씩) 반복되어 `P` 컬럼에 존재하기 때문.
        ```SQL
        CASE WHEN N NOT IN (SELECT DISTINCT P FROM BST WHERE P IS NOT NULL) THEN 'Leaf' 
        ```








