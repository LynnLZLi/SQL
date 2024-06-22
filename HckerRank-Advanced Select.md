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


