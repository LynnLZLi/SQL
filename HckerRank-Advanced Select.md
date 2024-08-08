# The PADS

Generate the following two result sets:

1. Query an alphabetically ordered list of all names in OCCUPATIONS, immediately followed by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S).
   
2. Query the number of ocurrences of each occupation in OCCUPATIONS. Sort the occurrences in ascending order, and output them in the following format:

There are a total of [occupation_count] [occupation]s.

where [occupation_count] is the number of occurrences of an occupation in OCCUPATIONS and [occupation] is the lowercase occupation name. If more than one Occupation has the same [occupation_count], they should be ordered alphabetically.

Note: There will be at least two entries in the table for each type of occupation.

**Input Format**

The OCCUPATIONS table is described as follows:

![img](https://s3.amazonaws.com/hr-challenge-images/12889/1443816414-2a465532e7-1.png)  

Occupation will only contain one of the following values: Doctor, Professor, Singer or Actor.

**Sample Input**

An OCCUPATIONS table that contains the following records:

![img](https://s3.amazonaws.com/hr-challenge-images/12889/1443816608-0b4d01d157-2.png)

```sql
(SELECT CONCAT(NAME, '(',SUBSTRING(Occupation,1,1),')') AS NAMEOCC 
 FROM OCCUPATIONS)

UNION ALL

(SELECT CONCAT('There are a total of ', count(occupation), ' ',LOWER(Occupation), CASE WHEN COUNT(Occupation)>1 THEN 's.' ELSE '.' END) AS Output 
FROM OCCUPATIONS 
GROUP BY OCCUPATION)

ORDER BY NAMEOCC;
```

# Occupations

Pivot the Occupation column in OCCUPATIONS so that each Name is sorted alphabetically and displayed underneath its corresponding Occupation. The output column headers should be Doctor, Professor, Singer, and Actor, respectively.

Note: Print NULL when there are no more names corresponding to an occupation.

**Input Format**

The OCCUPATIONS table is described as follows:

![img](https://s3.amazonaws.com/hr-challenge-images/12889/1443816414-2a465532e7-1.png)  

Occupation will only contain one of the following values: Doctor, Professor, Singer or Actor.

**Sample Input**

![img](https://s3.amazonaws.com/hr-challenge-images/12890/1443817648-1b2b8add45-2.png)  


```sql
SELECT
    MAX(CASE WHEN Occupation = 'Doctor' THEN Name END) AS Doctor,
    MAX(CASE WHEN Occupation = 'Professor' THEN Name END) AS Professor,
    MAX(CASE WHEN Occupation = 'Singer' THEN Name END) AS Singer,
    MAX(CASE WHEN Occupation = 'Actor' THEN Name END) AS Actor
FROM OCCUPATIONS
GROUP BY Name
ORDER BY Name;
```


## Note

Here's a detailed conceptual explanation in English to help you understand how the data is processed and transformed at each stage of the SQL query, given that I'm unable to run SQL queries or generate actual data tables directly.

### Initial Data
Assume your `OCCUPATIONS` table looks like this:

| Name       | Occupation |
|------------|------------|
| Samantha   | Doctor     |
| Julia      | Actor      |
| Maria      | Actor      |
| Meera      | Singer     |
| Ashely     | Professor  |
| Christeen  | Professor  |
| Jane       | Actor      |
| Jenny      | Doctor     |
| Priya      | Singer     |
| Ketty      | Professor  |

### Step 1: Applying ROW_NUMBER() Window Function
This step sorts and numbers the names within each occupational group:

```sql
SELECT Name, Occupation,
       ROW_NUMBER() OVER (PARTITION BY Occupation ORDER BY Name) as RowNum
FROM OCCUPATIONS
```

Assuming the results look like this:

| Name       | Occupation | RowNum |
|------------|------------|--------|
| Julia      | Actor      | 1      |
| Jane       | Actor      | 2      |
| Maria      | Actor      | 3      |
| Jenny      | Doctor     | 1      |
| Samantha   | Doctor     | 2      |
| Ashely     | Professor  | 1      |
| Christeen  | Professor  | 2      |
| Ketty      | Professor  | 3      |
| Meera      | Singer     | 1      |
| Priya      | Singer     | 2      |

### Step 2: Using Conditional Aggregation to Create a Pivot Table
This step aggregates data according to `RowNum`, creating a column for each occupation:

```sql
SELECT
  MAX(CASE WHEN Occupation = 'Doctor' THEN Name END) AS Doctor,
  MAX(CASE WHEN Occupation = 'Professor' THEN Name END) AS Professor,
  MAX(CASE WHEN Occupation = 'Singer' THEN Name END) AS Singer,
  MAX(CASE WHEN Occupation = 'Actor' THEN Name END) AS Actor
FROM (
  SELECT Name, Occupation,
         ROW_NUMBER() OVER (PARTITION BY Occupation ORDER BY Name) as RowNum
  FROM OCCUPATIONS
) AS SortedNames
GROUP BY RowNum
ORDER BY RowNum;
```

Assuming the final results are as follows:

| Doctor   | Professor | Singer | Actor  |
|----------|-----------|--------|--------|
| Jenny    | Ashely    | Meera  | Julia  |
| Samantha | Christeen | Priya  | Jane   |
| NULL     | Ketty     | NULL   | Maria  |

### Result Explanation
- **Each Row**: Corresponds to `RowNum`, indicating the alphabetical rank within each occupation. For example, the first row includes the first alphabetically sorted name under each occupation.
- **Each Column**: Represents an occupation, displayed as Doctor, Professor, Singer, Actor, as per your requirements.
- **NULL Values**: Indicate where an occupation does not have enough names to fill that row. For example, if there are only two doctors, then the third row under the Doctor column will be NULL.

### Why Use `CASE WHEN` and `MAX`

In our SQL query, the combination of `CASE WHEN` and `MAX` works like this:

1. **Role of `CASE WHEN`:**
   - This command checks each person's occupation.
   - If someone is a doctor, `CASE WHEN` selects this person's name; if not, it selects `NULL`.
   - This is done for each occupation, so for each person, `CASE WHEN` produces a name in the column corresponding to their occupation and `NULL` in all other columns.

2. **Role of `MAX`:**
   - After processing all the data, each column (each occupation) will have many `NULLs` and some names.
   - The `MAX` function serves to pick out the names from these `NULLs` and names (because in text, `NULL` is considered the least).
   - Thus, despite many `NULLs`, each occupational column ultimately displays only the names.

### Table Transformation

- Initially, the table is vertical, with each person's occupation and name on one row.
- After using `CASE WHEN` and `MAX`, we create four new columns (Doctor, Professor, Singer, Actor), each filled with names or `NULLs` corresponding to each occupation.
- This transforms the originally vertical list of occupations into a horizontal arrangement, where each occupation becomes a column in the table, and each row displays names from different occupations.

### The Role of Row Numbers

- By using `ROW_NUMBER()`, we assign a number to each name within their occupation, which helps us maintain the order of names and ensure that in the output table, each row correctly aligns names across various occupations.
- `GROUP BY RowNum` ensures that names with the same number appear on the same row.


# Binary Tree Nodes

You are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, and P is the parent of N.

![img](https://s3.amazonaws.com/hr-challenge-images/12888/1443818507-5095ab9853-1.png)

Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node:

Root: If node is root node.
Leaf: If node is leaf node.
Inner: If node is neither root nor leaf node.
Sample Input

![img](https://s3.amazonaws.com/hr-challenge-images/12888/1443818467-30644673f6-2.png)

Sample Output

1 Leaf
2 Inner
3 Leaf
5 Root
6 Leaf
8 Inner
9 Leaf

Explanation

The Binary Tree below illustrates the sample:

![img](https://s3.amazonaws.com/hr-challenge-images/12888/1443773633-f9e6fd314e-simply_sql_bst.png)

```sql
SELECT
    N,
    CASE
        WHEN P IS NULL THEN 'Root'
        WHEN N NOT IN (SELECT P FROM BST WHERE P IS NOT NULL) THEN 'Leaf'
        ELSE 'Inner'
    END AS NodeType
FROM
    BST
ORDER BY
    N;
```


# New Companies

Amber's conglomerate corporation just acquired some new companies. Each of the companies follows this hierarchy:

!https://s3.amazonaws.com/hr-challenge-images/19505/1458531031-249df3ae87-ScreenShot2016-03-21at8.59.56AM.png

Given the table schemas below, write a query to print the *company_code*, *founder* name, total number of *lead* managers, total number of *senior* managers, total number of *managers*, and total number of *employees*. Order your output by ascending *company_code*.

**Note:**

- The tables may contain duplicate records.
- The *company_code* is string, so the sorting should not be **numeric**. For example, if the *company_codes* are *C_1*, *C_2*, and *C_10*, then the ascending *company_codes* will be *C_1*, *C_10*, and *C_2*.

---

**Input Format**

The following tables contain company data:

- *Company:* The *company_code* is the code of the company and *founder* is the founder of the company.
    
    !https://s3.amazonaws.com/hr-challenge-images/19505/1458531125-deb0a57ae1-ScreenShot2016-03-21at8.50.04AM.png
    
- *Lead_Manager:* The *lead_manager_code* is the code of the lead manager, and the *company_code* is the code of the working company.
    
    !https://s3.amazonaws.com/hr-challenge-images/19505/1458534960-2c6d764e3c-ScreenShot2016-03-21at8.50.12AM.png
    
- *Senior_Manager:* The *senior_manager_code* is the code of the senior manager, the *lead_manager_code* is the code of its lead manager, and the *company_code* is the code of the working company.
    
    !https://s3.amazonaws.com/hr-challenge-images/19505/1458534973-6548194998-ScreenShot2016-03-21at8.50.21AM.png
    
- *Manager:* The *manager_code* is the code of the manager, the *senior_manager_code* is the code of its senior manager, the *lead_manager_code* is the code of its lead manager, and the *company_code* is the code of the working company.
    
    !https://s3.amazonaws.com/hr-challenge-images/19505/1458534988-7fc0af46ce-ScreenShot2016-03-21at8.50.29AM.png
    
- *Employee:* The *employee_code* is the code of the employee, the *manager_code* is the code of its manager, the *senior_manager_code* is the code of its senior manager, the *lead_manager_code* is the code of its lead manager, and the *company_code* is the code of the working company.
    
    !https://s3.amazonaws.com/hr-challenge-images/19505/1458535002-d47f63cbb4-ScreenShot2016-03-21at8.50.41AM.png
    

---

**Sample Input**

*Company*

Table:

!https://s3.amazonaws.com/hr-challenge-images/19505/1458535049-2a207c44b3-ScreenShot2016-03-21at8.50.52AM.png

*Lead_Manager*

Table:

!https://s3.amazonaws.com/hr-challenge-images/19505/1458535073-919107f639-ScreenShot2016-03-21at8.51.03AM.png

*Senior_Manager*

Table:

!https://s3.amazonaws.com/hr-challenge-images/19505/1458535111-b1c48335b3-ScreenShot2016-03-21at8.51.15AM.png

*Manager*

Table:

!https://s3.amazonaws.com/hr-challenge-images/19505/1458535122-888f4bf340-ScreenShot2016-03-21at8.51.26AM.png

*Employee*

Table:

!https://s3.amazonaws.com/hr-challenge-images/19505/1458535134-878767e0d9-ScreenShot2016-03-21at8.51.52AM.png

**Sample Output**

`C1 Monika 1 2 1 2
C2 Samantha 1 1 2 2`

**Explanation**

In company *C1*, the only lead manager is *LM1*. There are two senior managers, *SM1* and *SM2*, under *LM1*. There is one manager, *M1*, under senior manager *SM1*. There are two employees, *E1* and *E2*, under manager *M1*.

In company *C2*, the only lead manager is *LM2*. There is one senior manager, *SM3*, under *LM2*. There are two managers, *M2* and *M3*, under senior manager *SM3*. There is one employee, *E3*, under manager *M2*, and another employee, *E4*, under manager, *M3*.

```sql
select c.company_code, c.founder,
       count(distinct l.lead_manager_code),
       count(distinct s.senior_manager_code),
       count(distinct m.manager_code),
       count(distinct e.employee_code)
from Company as c 
join Lead_Manager as l 
on c.company_code = l.company_code
join Senior_Manager as s
on l.lead_manager_code = s.lead_manager_code
join Manager as m 
on m.senior_manager_code = s.senior_manager_code
join Employee as e
on e.manager_code = m.manager_code
group by c.company_code, c.founder
order by c.company_code;
```
