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


# 还差一道题没写完
