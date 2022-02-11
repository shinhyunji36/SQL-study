# HackerRank - SQL 문제풀이
## New Companies

### 문제
Amber's conglomerate corporation just acquired some new companies. Each of the companies follows this hierarchy: <br>
  ![image](https://user-images.githubusercontent.com/74661937/153428415-4859d80a-8bf0-42a9-860c-f73d2e558095.png)


Given the table schemas below, write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, total number of managers, and total number of employees. Order your output by ascending company_code.

Note:
  - The tables may contain duplicate records.
  - The company_code is string, so the sorting should not be numeric.<br>
    For example, if the company_codes are C_1, C_2, and C_10, then the ascending company_codes will be C_1, C_10, and C_2.

**Input Format**

The following tables contain company data:

- Company : The company_code is the code of the company and founder is the founder of the company. <br>
  ![image](https://user-images.githubusercontent.com/74661937/153429281-24b7530b-2f63-4731-b993-98f90d7828a7.png)


- Lead_Manager : The lead_manager_code is the code of the lead manager, and the company_code is the code of the working company.<br>
  ![image](https://user-images.githubusercontent.com/74661937/153429339-5890d799-83a8-4942-81b0-16516674fad8.png)

- Senior_Manager : The senior_manager_code is the code of the senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company. <br>
  ![image](https://user-images.githubusercontent.com/74661937/153429505-1db9eeb5-8a82-4676-af0b-a5566909ccb1.png)


- Manager : The manager_code is the code of the manager, the senior_manager_code is the code of its senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company.<br>
  ![image](https://user-images.githubusercontent.com/74661937/153429548-2ea515e4-83a6-45f8-9055-b188f608cea7.png)

- Employee : The employee_code is the code of the employee, the manager_code is the code of its manager, the senior_manager_code is the code of its senior manager, the lead_manager_code is the code of its lead manager, and the company_code is the code of the working company. <br>
  ![image](https://user-images.githubusercontent.com/74661937/153429652-e16e0cf1-2d1c-4831-a1cb-b87014cdf161.png)


**Sample Input**
- Company table<br>
  ![image](https://user-images.githubusercontent.com/74661937/153429931-9046e821-32f4-4898-8939-19e0a3542e98.png)

- Lead_Manager table<br>
  ![image](https://user-images.githubusercontent.com/74661937/153429963-51ffcf27-c5b9-4a22-8823-dad1880f6875.png)
  

- Senior_Manager table<br>
  ![image](https://user-images.githubusercontent.com/74661937/153430009-cfc5035d-e83e-4ffd-bec1-c19b952c74e5.png)

- Manager table<br>
  ![image](https://user-images.githubusercontent.com/74661937/153430047-88c62587-fb37-4255-8c3b-0575b3c37c57.png)

- Employee table<br>
  ![image](https://user-images.githubusercontent.com/74661937/153430074-30745e86-0faa-431d-bcdb-cf37206bde18.png)



**Sample Output**<br>
```
C1 Monika 1 2 1 2
C2 Samantha 1 1 2 2
```

**Explanation**

In company C1, the only lead manager is LM1. <br>
There are two senior managers, SM1 and SM2, under LM1. <br>
There is one manager, M1, under senior manager SM1. <br>
There are two employees, E1 and E2, under manager M1. <br>
In company C2, the only lead manager is LM2. <br>
There is one senior manager, SM3, under LM2. <br>
There are two managers, M2 and M3, under senior manager SM3. <br>
There is one employee, E3, under manager M2, and another employee, E4, under manager, M3.



### 풀이
  ```sql
  SELECT c.company_code, c.founder,
         COUNT(DISTINCT lm.lead_manager_code),
         COUNT(DISTINCT sm.senior_manager_code),
         COUNT(DISTINCT m.manager_code),
         COUNT(DISTINCT e.employee_code)
  FROM Company c
  LEFT JOIN Lead_Manager lm ON c.company_code = lm.company_code
  LEFT JOIN Senior_Manager sm ON lm.lead_manager_code = sm.lead_manager_code
  LEFT JOIN Manager m ON sm.senior_manager_code = m.senior_manager_code
  LEFT JOIN Employee e ON m.manager_code = e.manager_code

  GROUP BY c.company_code, c.founder
  ORDER BY c.company_code
  ```

  ![image](https://user-images.githubusercontent.com/74661937/153612178-7952daf3-7498-409b-893d-06a0842835c9.png)


