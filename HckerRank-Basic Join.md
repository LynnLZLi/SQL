# Africa Cities

Given the **CITY** and **COUNTRY** tables, query the names of all cities where the *CONTINENT* is *'Africa'*.

**Note:** *CITY.CountryCode* and *COUNTRY.Code* are matching key columns.

**Input Format**

The **CITY** and **COUNTRY** tables are described as follows:

![img](https://s3.amazonaws.com/hr-challenge-images/8137/1449729804-f21d187d0f-CITY.jpg)

![img](https://s3.amazonaws.com/hr-challenge-images/8342/1449769013-e54ce90480-Country.jpg)

```sql
SELECT CITY.NAME
FROM CITY
JOIN COUNTRY ON CITY.COUNTRYCODE = COUNTRY.CODE
WHERE COUNTRY.CONTINENT = 'Africa';
```

# Population Sensus

Given the **CITY** and **COUNTRY** tables, query the sum of the populations of all cities where the *CONTINENT* is *'Asia'*.

**Note:** *CITY.CountryCode* and *COUNTRY.Code* are matching key columns.

**Input Format**

The **CITY** and **COUNTRY** tables are described as follows:

![img](https://s3.amazonaws.com/hr-challenge-images/8137/1449729804-f21d187d0f-CITY.jpg)

![img](https://s3.amazonaws.com/hr-challenge-images/8342/1449769013-e54ce90480-Country.jpg)

```sql
SELECT SUM(CITY.POPULATION)
FROM CITY
JOIN COUNTRY ON CITY.COUNTRYCODE = COUNTRY.CODE
WHERE COUNTRY.CONTINENT = 'Asia';
```

# Average Population of Each Continent

Given the **CITY** and **COUNTRY** tables, query the names of all the continents (*COUNTRY.Continent*) and their respective average city populations (*CITY.Population*) rounded *down* to the nearest integer.

**Note:** *CITY.CountryCode* and *COUNTRY.Code* are matching key columns.

**Input Format**

The **CITY** and **COUNTRY** tables are described as follows:

![img](https://s3.amazonaws.com/hr-challenge-images/8137/1449729804-f21d187d0f-CITY.jpg)

![img](https://s3.amazonaws.com/hr-challenge-images/8342/1449769013-e54ce90480-Country.jpg)

```sql
SELECT COUNTRY.CONTINENT, FLOOR(AVG(CITY.POPULATION))
FROM CITY
JOIN COUNTRY ON CITY.COUNTRYCODE = COUNTRY.CODE
GROUP BY COUNTRY.CONTINENT;
```

# The Report

You are given two tables: *Students* and *Grades*. *Students* contains three columns *ID*, *Name* and *Marks*.

![img](!https://s3.amazonaws.com/hr-challenge-images/12891/1443818166-a5c852caa0-1.png)

*Grades* contains the following data:

![img](!https://s3.amazonaws.com/hr-challenge-images/12891/1443818137-69b76d805c-2.png)

*Ketty* gives *Eve* a task to generate a report containing three columns: *Name*, *Grade* and *Mark*. *Ketty* doesn't want the NAMES of those students who received a grade lower than *8*. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.

Write a query to help Eve.

**Sample Input**

![img](!https://s3.amazonaws.com/hr-challenge-images/12891/1443818093-b79f376ec1-3.png)

**Sample Output**

```
Maria 10 99
Jane 9 81
Julia 9 88
Scarlet 8 78
NULL 7 63
NULL 7 68
```

**Note**

Print "NULL"  as the name if the grade is less than 8.

**Explanation**

Consider the following table with the grades assigned to the students:

![img](!https://s3.amazonaws.com/hr-challenge-images/12891/1443818026-0b3af8db30-4.png)

So, the following students got *8*, *9* or *10* grades:

- *Maria (grade 10)*
- *Jane (grade 9)*
- *Julia (grade 9)*
- *Scarlet (grade 8)*



1. **Basic Task**: Eve is tasked with generating a report that includes three columns: Name, Grade, and Mark.

2. **Rules for Displaying Names**:
   - If a student's grade is **below 8**, their actual name should not be shown; instead, "NULL" should be displayed.
   - If a student's grade is **8 or higher**, their actual name should be displayed.

3. **Sorting Rules**:
   - **Primary Sorting Rule**: The report should be sorted by Grade in **descending order**. This means students with higher grades are listed first.
   - **Secondary Sorting Rules**:
     - For students with grades **8-10**, if multiple students share the same grade, they should be ordered alphabetically by their names.
     - For students with grades **1-7**, if multiple students share the same grade, they should be ordered by their marks in **ascending order**.

```sql
SELECT
    CASE
        WHEN G.GRADE < 8 THEN 'NULL'
        ELSE S.NAME
    END AS NAME,
    G.GRADE,
    S.MARKS
FROM STUDENTS S
JOIN GRADES G ON S.MARKS BETWEEN G.MIN_MARK AND G.MAX_MARK
ORDER BY
    G.GRADE DESC,
    CASE
        WHEN G.GRADE >= 8 THEN S.NAME
        ELSE NULL
    END ASC,
    CASE
        WHEN G.GRADE < 8 THEN S.MARKS
        ELSE NULL
    END ASC;
```

# Top Competitors

Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective *hacker_id* and *name* of hackers who achieved full scores for *more than one* challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending *hacker_id*.

---

**Input Format**

The following tables contain contest data:

- *Hackers:* The *hacker_id* is the id of the hacker, and *name* is the name of the hacker.
    
![img](!https://s3.amazonaws.com/hr-challenge-images/19504/1458526776-67667350b4-ScreenShot2016-03-21at7.45.59AM.png)
    
- *Difficulty:* The *difficult_level* is the level of difficulty of the challenge, and *score* is the score of the challenge for the difficulty level.
    
![img](!https://s3.amazonaws.com/hr-challenge-images/19504/1458526915-57eb75d9a2-ScreenShot2016-03-21at7.46.09AM.png)
    
- *Challenges:* The *challenge_id* is the id of the challenge, the *hacker_id* is the id of the hacker who created the challenge, and *difficulty_level* is the level of difficulty of the challenge.
    
![img](!https://s3.amazonaws.com/hr-challenge-images/19504/1458527032-f9ca650442-ScreenShot2016-03-21at7.46.17AM.png)
    
- *Submissions:* The *submission_id* is the id of the submission, *hacker_id* is the id of the hacker who made the submission, *challenge_id* is the id of the challenge that the submission belongs to, and *score* is the score of the submission.
    
![img](!https://s3.amazonaws.com/hr-challenge-images/19504/1458527077-298f8e922a-ScreenShot2016-03-21at7.46.29AM.png)
    

---

**Sample Input**

*Hackers*

Table:

![img](!https://s3.amazonaws.com/hr-challenge-images/19504/1458527241-6922b4ad87-ScreenShot2016-03-21at7.47.02AM.png)

*Difficulty*

Table:

![img](!https://s3.amazonaws.com/hr-challenge-images/19504/1458527265-7ad6852a13-ScreenShot2016-03-21at7.46.50AM.png)

*Challenges*

Table:

![img](!https://s3.amazonaws.com/hr-challenge-images/19504/1458527285-01e95eb6ec-ScreenShot2016-03-21at7.46.40AM.png)

*Submissions*

Table:

![img](!https://s3.amazonaws.com/hr-challenge-images/19504/1458527812-479a74b99f-ScreenShot2016-03-21at8.06.05AM.png)

**Sample Output**

`90411 Joe`

```sql
SELECT H.hacker_id, H.name
FROM Submissions S
JOIN Challenges C ON S.challenge_id = C.challenge_id
JOIN Difficulty D ON C.difficulty_level = D.difficulty_level
JOIN Hackers H ON S.hacker_id = H.hacker_id
WHERE S.score = D.score
GROUP BY H.hacker_id, H.name
HAVING COUNT(S.hacker_id) > 1
ORDER BY COUNT(S.hacker_id) DESC, H.hacker_id ASC
```

# Ollivander's Inventory

Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand.

Hermione decides the best way to choose is by determining the minimum number of gold galleons needed to buy each *non-evil* wand of high power and age. Write a query to print the *id*, *age*, *coins_needed*, and *power* of the wands that Ron's interested in, sorted in order of descending *power*. If more than one wand has same power, sort the result in order of descending *age*.

**Input Format**

The following tables contain data on the wands in Ollivander's inventory:

- *Wands:* The *id* is the id of the wand, *code* is the code of the wand, *coins_needed* is the total number of gold galleons needed to buy the wand, and *power* denotes the quality of the wand (the higher the power, the better the wand is).
    
![img](!https://s3.amazonaws.com/hr-challenge-images/19502/1458538092-b2a8163a74-ScreenShot2016-03-08at12.13.39AM.png)
    
- *Wands_Property:* The *code* is the code of the wand, *age* is the age of the wand, and *is_evil* denotes whether the wand is good for the dark arts. If the value of *is_evil* is *0*, it means that the wand is not evil. The mapping between *code* and *age* is one-one, meaning that if there are two pairs,  and , then  and .
    
![img](!https://s3.amazonaws.com/hr-challenge-images/19502/1458538221-18c4092b7d-ScreenShot2016-03-08at12.13.53AM.png)

---

**Sample Input**

*Wands*

Table:

![img](!https://s3.amazonaws.com/hr-challenge-images/19502/1458538559-51bf29644e-ScreenShot2016-03-21at10.34.41AM.png)

*Wands_Property*

Table:

![img](!https://s3.amazonaws.com/hr-challenge-images/19502/1458538583-fd514566f9-ScreenShot2016-03-21at10.34.28AM.png)

**Sample Output**

`9 45 1647 10
12 17 9897 10
1 20 3688 8
15 40 6018 7
19 20 7651 6
11 40 7587 5
10 20 504 5
18 40 3312 3
20 17 5689 3
5 45 6020 2
14 40 5408 1`


```sql
SELECT w.id, wp.age, w.coins_needed, w.power
FROM Wands w
JOIN Wands_Property wp ON w.code = wp.code
WHERE wp.is_evil = 0
AND w.coins_needed = (
    SELECT MIN(w2.coins_needed)
    FROM Wands w2
    JOIN Wands_Property wp2 ON w2.code = wp2.code
    WHERE wp2.age = wp.age AND wp2.is_evil = 0 AND w2.power = w.power
)
ORDER BY w.power DESC, wp.age DESC;
```

# Challenges

Julia asked her students to create some coding challenges. Write a query to print the *hacker_id*, *name*, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by *hacker_id*. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.

**Input Format**

The following tables contain challenge data:

- *Hackers:* The *hacker_id* is the id of the hacker, and *name* is the name of the hacker.
    
![img](!https://s3.amazonaws.com/hr-challenge-images/19506/1458521004-cb4c077dd3-ScreenShot2016-03-21at6.06.54AM.png)
    
- *Challenges:* The *challenge_id* is the id of the challenge, and *hacker_id* is the id of the student who created the challenge.
    
![img](!https://s3.amazonaws.com/hr-challenge-images/19506/1458521079-549341d9ec-ScreenShot2016-03-21at6.07.03AM.png)
    

---

**Sample Input 0**

*Hackers*

Table:

![img](!https://s3.amazonaws.com/hr-challenge-images/19506/1458521384-34c6866dae-ScreenShot2016-03-21at6.07.15AM.png)

*Challenges*

Table:

![img](!https://s3.amazonaws.com/hr-challenge-images/19506/1458521410-befa8e1cd9-ScreenShot2016-03-21at6.07.25AM.png)

**Sample Output 0**

```
21283 Angela 6
88255 Patrick 5
96196 Lisa 1

```

**Sample Input 1**

*Hackers*

Table:

![img](!https://s3.amazonaws.com/hr-challenge-images/19506/1458521469-87036deea3-ScreenShot2016-03-21at6.07.48AM.png)

*Challenges*

Table:

![img](!https://s3.amazonaws.com/hr-challenge-images/19506/1458521490-358215cf0b-ScreenShot2016-03-21at6.07.58AM.png)

**Sample Output 1**

```
12299 Rose 6
34856 Angela 6
79345 Frank 4
80491 Patrick 3
81041 Lisa 1
```

```SQL
select m.hacker_id, h.name, sum(score) as total_score from
(select hacker_id, challenge_id, max(score) as score
from Submissions group by hacker_id, challenge_id) as m
join Hackers as h
on m.hacker_id = h.hacker_id
group by m.hacker_id, h.name
having total_score > 0
order by total_score desc, m.hacker_id;
```


# vosynverse-backend

This repository is to store any code, README or other files made by the backend team.

## Readme Files
- [Tech Stack & Tools](readme_files/Tech_Stack_Tools.md)
- [Names & Syntax](readme_files/Nomenclature_Syntax.md)
- [Model Documentation](readme_files/Models_Docs.md)


# Local Environment Setup
### Django Project Setup
1. Clone this repository
2. Create a virtual environment
    > python -m venv .venv
3. Activate the virtual environment (Windows PowerShell)
    > .venv\Scripts\activate
4. Install the required packages
    > pip install -r requirements.txt


### PostgreSQL Database Setup and Configuration
1. Install PostgreSQL along pgAgent (using Stack Builder),
2. Login in the SQL Shell as superuser (psql) [[1]](https://docs.djangoproject.com/en/4.2/ref/databases/#optimizing-postgresql-s-configuration) [[2]](https://djangocentral.com/using-postgresql-with-django/) and then run the following:
    ```sql
    CREATE DATABASE vosynverse_db;
    CREATE USER vvuser WITH ENCRYPTED PASSWORD 'development-env-password';

    <!-- Optimizing PostgreSQL's Configuration-->
    ALTER ROLE vvuser SET client_encoding TO 'utf8';
    ALTER ROLE vvuser SET default_transaction_isolation TO 'read committed';
    ALTER ROLE vvuser SET timezone TO 'UTC';

    <!-- GRANT ALL PRIV.. might not be needed if we are changing the owner but I haven't checked -->
    GRANT ALL PRIVILEGES ON DATABASE vosynverse_db TO vvuser;
    ALTER DATABASE vosynverse_db OWNER TO vvuser;

    <!-- Need to be able to create databases for testing -->
    ALTER USER vvuser CREATEDB;
    ```
3. Use or copy the config/config.ini.example file to create a config.ini file in the same directory
4. Edit the config.ini file to match the database name, user and password created in the previous steps (or copy the below example for now)
    - config.ini:
        ```ini
        [Django]
        SECRET_KEY = django-insecure-k-mdzug-&s-^@d3*#+2mymwp^z^j@0!ivwrt*e4g10=(ge0+yf
        DEBUG = True
        ALLOWED_HOSTS = *
        CORS_ALLOW_ALL_ORIGINS = True
        
        [PostgreSQL]
        DBNAME = vosynverse_db
        HOST = localhost
        PORT = 5432
        USER = vvuser
        PASSWORD = development-env-password
        ```

### Django Project Setup Continued
1. Migration
    > python manage.py migrate
2. Run the server for testing (May not work without setting up typesense, see instructions below)
    > python manage.py runserver
3. Run the tests
    > python manage.py test
4. Note: Whenever there are changes in the model (i.e. added new fields for the video model), make sure to run this and then perform step 1:
    > python manage.py makemigrations
 

## Expected Starting Project Structure

Note: (might not be updated)
```
vosynverse-backend [root / this folder]
├── .venv [ignored in git]
│   └── ... [virtual environment files]
├── config
│   ├── config.ini
│   └── config.ini.example
├── readme_files
│   └── ... [readme files]
├── vv_backend
│   ├── content
│   │   └── ... [content app files]
│   ├── user_interactions
│   │   └── ... [user_interactions app files]
│   ├── users
│   │   └── ... [users app files]
│   ├── vv_backend
│   │   ├── __init__.py
│   │   ├── asgi.py
│   │   ├── settings.py
│   │   ├── urls.py
│   │   └── wsgi.py
│   └── manage.py
├── .gitignore
├── README.md
└── requirements.txt
```

## Typesense Installation
1. To run the project, you must have typesense running locally. 
2. Follow this guide: https://typesense.org/docs/guide/install-typesense.html
3. After installation, you should be able to run this on your terminal:
   - ```curl http://localhost:8108/health```
   - And get the response: {"ok":true}
4. Make sure that the settings from your typesense config file matches the config.ini file in the project, especially the API key.
5. Now before you run the backend server, you must run the following commands:
   - ```python manage.py typesense_init```
   - ```python manage.py typesense_index```
6. Run the backend server as expected:
   - ```python manage.py runserver```


## Load Sample Data 
1. To load sample video models
   - ```python manage.py loaddata test_user```
2. To load sample user models
   - ```python manage.py loaddata 239_videos```
3. To load sample playlist models
   - ```python manage.py loaddata test_playlists```


## Miscellaneous Commands
1. Django admin panel access
   - ```python manage.py makesuperuser```
   - Input email/username/password
Now you can login to django admin through: http://localhost:8000/admin/
2. Export fixture files (will export from your db to the file below)
   - ```python -Xutf8 manage.py dumpdata --format json -o .\content\fixtures\NAME.json --traceback content```
   - To load this file back into the db:
   - ```python manage.py loaddata NAME```
3. To download and process videos and then upload them to the S3 bucket.
   - Make sure to set 'USE_S3' = False in the config.ini file if you are only testing the command.
   - Edit the list of videos in content/management/commands/download_and_process_videos.py. Then run:
   - ```python manage.py download_and_process_videos```
