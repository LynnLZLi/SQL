## Weather Observation Station 5  
  
Query the two cities in **STATION** with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.  
  
The **STATION** table is described as follows:  
  
![img](https://s3.amazonaws.com/hr-challenge-images/9336/1449345840-5f0a551030-Station.jpg)  
  
where **LAT_N** is the northern latitude and **LONG_W** is the western longitude.

```sql
(
  SELECT CITY, LENGTH(CITY) AS LEN
  FROM STATION
  ORDER BY LEN ASC, CITY ASC
  LIMIT 1
)
UNION ALL
(
  SELECT CITY, LENGTH(CITY) AS LEN
  FROM STATION
  ORDER BY LEN DESC, CITY ASC
  LIMIT 1
);
```
