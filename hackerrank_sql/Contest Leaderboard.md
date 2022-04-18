# HackerRank - SQL 문제풀이

## Contest Leaderboard
- [문제 링크](https://www.hackerrank.com/challenges/contest-leaderboard/problem?isFullScreen=true)
- 여러 hacker들이 여러 challenge에 참여해 점수를 받았다.
  - 이 때, 각 hacker들이 각 challenge에서 받은 가장 높은 점수를 합산해서 `total_score`로 지정하고
  - 이를 기준으로 내림차순(점수가 가장 높은 사람이 맨 위에 랭크되는) Contest leaderboard를 작성한다.
  - 같은 `total_socore`가 나왔을 경우에는 `hacker_id`를 오름차순으로 정렬한다.
  - `total_score`가 0인 hacker는 제외한다.
   
### 나의 풀이
- 컨셉
  - `hacker_id`를 기준으로 Hackers 테이블과 Submissions 테이블을 `INNER JOIN` 한다. (`on` 잊지 말기)
    - 약칭 때문에 맨 첫줄의 `SELECT`를 작성하기 전에, 먼저 INNER JOIN 부터 작성하는 것이 편하다.
  - `GROUP BY`를 사용한 후
    - 집계 함수를 여러개 사용할 수 없다. → 서브 쿼리 활용 필요
    - 조건을 달기 위해서 `HAVING` 사용하기


- 코드 
  ```python
  SELECT h.hacker_id, h.name, SUM(s.score) AS total_score
  FROM HACKERS h
  INNER JOIN (SELECT hacker_id, MAX(score) AS score
              FROM SUBMISSIONS
              GROUP BY hacker_id, challenge_id) s
  ON h.hacker_id = s.hacker_id
  GROUP BY h.name, h.hacker_id HAVING total_score > 0
  ORDER BY total_score DESC, h.hacker_id ASC;
  
  ```
  ```
  [sample output]
  4071 Rose 191
  74842 Lisa 174
  84072 Bonnie 100
  4806 Angela 89
  26071 Frank 85
  80305 Kimberly 67
  49438 Patrick 43
  ```
  
  
  - 서브 쿼리 살펴보기 (inner join)
    - 각 hacker가 challenge 별로 받았던 최고점이 출력된다.
    
  ```python
  SELECT hacker_id, MAX(score) AS score
  FROM SUBMISSIONS
  GROUP BY hacker_id, challenge_id;
  ```
  ```
  [output]
  486 45 
  486 29 
  597 107 
  775 31 
  964 55 
  1700 66 
  1700 49 
  1746 32 
  1755 58 
  ```
