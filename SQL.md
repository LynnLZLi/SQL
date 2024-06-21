# Revising the Select Query I  
  
Query all columns for all American cities in CITY with populations larger than 100000. The CountryCode for America is USA.  
  
***Input Format***  
  
The CITY table is described as follows:  
  
![img](https://s3.amazonaws.com/hr-challenge-images/9336/1449345840-5f0a551030-Station.jpg)  
  
	SELECT * FROM city WHERE population > 100000 AND countrycode = 'USA';  
  
  
  
# Revising the Select Query II  
  
Query the names of all American cities in CITY with populations larger than 120000. The CountryCode for America is USA.  
  
	SELECT name FROM city WHERE countrycode = 'USA' AND population > 120000;  
  
  
  
# Select All  
  
Query all columns (attributes) for every row in the CITY table.  
  
	SELECT * FROM city;  
  