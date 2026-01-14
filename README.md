CREATE DATABASE IF NOT EXISTS COLLEGE1;  -- data base created
USE COLLEGE1;   -- data base used
CREATE TABLE STUDENT1 (  -- table created
ROLL_NO INT PRIMARY KEY,
NAME VARCHAR(50),
MARKS INT NOT NULL,
GRADE VARCHAR (1),
CITY VARCHAR (20)
);

CREATE TABLE DEPARTMENT(     # PARENT TABLE
ID INT PRIMARY KEY,
NAME VARCHAR(20)
);
INSERT INTO DEPARTMENT VALUES   -- values inserted in table
(101, "ENGLISH"),
(102, "IT");
SELECT * FROM DEPARTMENT;

UPDATE DEPARTMENT  -- update department with new record 
SET ID = 111
WHERE ID = 103;

SELECT * FROM DEPARTMENT;

CREATE TABLE TEACHER(    # CHILD TABLE
ID INT PRIMARY KEY,
NAME VARCHAR (50),
DEPT_ID INT,
FOREIGN KEY (DEPT_ID) REFERENCES DEPARTMENT(ID)
ON DELETE CASCADE  # whenever threir is change in any row if we need that bound to happen in other tables rows , we must use cascade 
ON UPDATE CASCADE
);
select * from teacher;

INSERT INTO TEACHER VALUES   -- insert values in table
(101, "ADAM", 101),
(102, "EVE", 102);
SELECT * FROM TEACHER; 

INSERT INTO STUDENT1 (ROLL_NO, NAME, MARKS, GRADE,CITY)   # values inserted in student1 table
VALUES
(101, "ANIL", 78, "C", "PUNE"),(102, "BHUMIKA", 93, "A", "MUMBAI"),
(103,"CHETAN ", 85, "B", "MUMBAI"), (104, "DHRUV", 96,"A", "DELHI"),
(105,"EMANUAL",12,"F", "DELHI"), (106,"FARAH", 82,"B","DELHI");
SELECT * FROM STUDENT;  # to see all the table
SELECT CITY FROM STUDENT;  # to see only city column 
SELECT DISTINCT CITY FROM STUDENT;  # to see only distinct cities
SELECT * FROM STUDENT WHERE MARKS>80; # get only above 80 marks student
SELECT * FROM STUDENT WHERE CITY="MUMBAI" ;  # get only mumbai students
SELECT * FROM STUDENT WHERE CITY="DELHI" AND MARKS<80; # get only delhi students with marks > 80
SELECT * FROM STUDENT WHERE CITY="MUMBAI" OR  MARKS >90; # get students who are in mumbai or marks >90
SELECT * FROM STUDENT WHERE MARKS BETWEEN 80 AND 90;  # get students whose marks between 80 and 90
SELECT * FROM STUDENT WHERE CITY IN ("DELHI", "MUUMBAI","GURGAON");
SELECT * FROM STUDENT WHERE CITY NOT IN ("MUMBAI","DELHI");  # get students who are ont from given cities
SELECT (NAME) FROM STUDENT LIMIT 3;  # get first 3 students in table
SELECT * FROM STUDENT 
WHERE MARKS >75
LIMIT 2;  # gives only 2 students whose marks  > 75 but according to table given
SELECT * FROM STUDENT
WHERE MARKS > 75
LIMIT 3; 
SELECT * FROM STUDENT
WHERE MARKS > 75; # gives all students whose marks > 75
SELECT * FROM STUDENT
ORDER BY CITY ASC; 
SELECT * FROM STUDENT ORDER BY MARKS ASC;
SELECT  * FROM STUDENT ORDER BY MARKS DESC;
SELECT * FROM STUDENT ORDER BY CITY DESC;
SELECT * 
FROM STUDENT 
ORDER BY MARKS DESC
LIMIT 3;
SELECT * FROM STUDENT ORDER BY MARKS ASC LIMIT 3;

SELECT MARKS FROM STUDENT; #  gives only marks table
SELECT MAX(MARKS) FROM STUDENT;
SELECT MIN(MARKS) FROM STUDENT;
SELECT AVG(MARKS) FROM STUDENT;
SELECT COUNT(NAME) FROM STUDENT;
SELECT SUM(MARKS) FROM STUDENT;

SELECT CITY, COUNT(NAME)
FROM STUDENT
GROUP BY CITY; # gives how many students from each city

SELECT CITY,NAME, 
COUNT(ROLL_NO) 
FROM STUDENT  
GROUP BY CITY, NAME;
SELECT CITY, AVG(MARKS)
FROM STUDENT
GROUP BY CITY; # group done by city and given average marks
SELECT CITY , NAME, MAX(MARKS)
FROM STUDENT GROUP BY CITY, NAME
LIMIT 4; -- group by city and gives city name and maximum marks of 4 students
SELECT CITY, AVG(MARKS)
FROM STUDENT
GROUP BY CITY
ORDER BY AVG(MARKS) ; # same as above but gives marks ascending

SELECT CITY
FROM STUDENT
WHERE GRADE = "A"
GROUP BY CITY
HAVING MAX(MARKS) >= 90
ORDER BY CITY ASC; -- returns only CITY names for cities having at least one "A" grade student with MAX(MARKS) ≥ 90, ordered alphabetically.​

SET SQL_SAFE_UPDATES = 0;  #It disables MySQL's safe updates mode for the current session.

UPDATE STUDENT
SET GRADE = "O"
WHERE GRADE = "A";  # where is A grade , write o
SELECT * FROM STUDENT;

UPDATE STUDENT 
SET MARKS = 82
WHERE ROLL_NO = 105; -- change 105 's marks to 82

UPDATE STUDENT
SET GRADE = "B"
WHERE MARKS BETWEEN 80 AND 90; #condition given


# WE HAVE GIVEN A WRONG QUESTION SO INCREASING ALL MARKS BY 1

UPDATE STUDENT  # situation where you have given wrong question in exam and want to give  grase 1 mark to all the kids
SET MARKS= MARKS + 1;
#WHERE MARKS >0;   CAN OR CAN NOT WRITE THIS LAST STATEMENT



UPDATE STUDENT 
SET MARKS = 12
WHERE ROLL_NO= 105;
SELECT * FROM  STUDENT;

# DELETE IS TO DELETE BETWEEN ROWS ,  BE CAREFULL WHILE DELETING ROWS AS WE CANT RETRIVE THAT DATA
DELETE FROM STUDENT   # This query deletes all rows from the STUDENT table where the MARKS column value is less than 35.
WHERE MARKS < 35;
SELECT * FROM STUDENT;

# JOIN IS USED TO COMBINE ROWS FROM TWO OR MORE TABLES, BASED ON RELATED COLUMNS BETWEEN THEM
# NO NEED OF FOREIGN KEY TO PERFORM JOINS ON TABLE

CREATE DATABASE IF NOT EXISTS STUDENT2;
USE STUDENT2;
CREATE TABLE student2 
( id INT (3) PRIMARY KEY,
name VARCHAR (20)
);
INSERT INTO  student2 VALUES 
(101, "adam"),
(102, "bob"),
(103, "casey")
;
SELECT * FROM student2;

CREATE TABLE if not exists course
(id INT (3),
course VARCHAR (20)
); 
INSERT INTO course values
(102, "english"),
(105, "math"),
(103, "science"),
(107, "computer");
SELECT * FROM course;

SELECT * 
FROM student as s   # here concept of alias is used
INNER JOIN course as c
ON s.id = c.id;  


# Left Join returns all records from left A table and matched records from right side B table 
SELECT * 
FROM student as s
LEFT JOIN course as c 
ON s.id = c.id;

# right join returns all records fron right side B column and matched records fron left side A table
SELECT * 
FROM student AS s
RIGHT JOIN course AS c
ON s.id = c.id;
SELECT * FROM student;

# FULL OUTER JOIN RETURNS ALL RECORDS WHEN THERE IS A MATCH IN EITHER LEFT OR RIGHT TABLE
SELECT * FROM student2 AS a
LEFT JOIN course AS b 
on a.id = b. id
UNION   # this is indirect full join
SELECT * FROM student2 AS a
RIGHT JOIN course AS b
ON a.id = b.id; 

 # LEFT EXCLUSIVE JOIN returns records from left table excluding matched record
 SELECT * 
 FROM student2 AS a
 LEFT JOIN course AS b
 ON a.id = b. id
 WHERE b.id IS NULL;
 
 # RIGHT EXCLUSIVE JOIN returns from right table records excluding matched records from both tables.
 SELECT * 
 FROM student2 as a
 RIGHT JOIN course AS b
 ON a.id = b.id
 WHERE a.id IS NULL;
 
 # FULL EXCLUSIVE JOIN returns all records other than matched records of two tables.
 SELECT * 
 FROM student2 as a
 LEFT JOIN course AS b
 ON a.id = b.id
 WHERE b.id IS NULL
 union 
 SELECT * 
 FROM student2 AS a
 RIGHT JOIN course AS b
 ON a.id = b.id
 WHERE a.id IS NULL;
 
# SELF JOIN returns records of single table and compare any two rows within same table
CREATE TABLE IF NOT EXISTS teacher (
id INT (3) PRIMARY KEY,
name VARCHAR(50),
dept_id INT(3)
);
INSERT INTO teacher VALUES
(101, "adam", 103),
(102, "bob", 104),
(103, "casey", null),
(104, "donald",103);
SELECT * FROM teacher;

SELECT a.name AS BOSS_NAME, b.name
FROM teacher AS a
join teacher As b
on a.id = b.dept_id;

SELECT name FROM teacher 
UNION   # it gives only unique
SELECT name FROM teacher;

SELECT name FROM teacher 
UNION ALL   # it gives duplicates also
SELECT name FROM teacher;

#SQL sub Queries : query within query
CREATE TABLE stud (
rollno INT (1) PRIMARY KEY,
name VARCHAR(50),
marks INT (3)
);
INSERT INTO stud VALUES 
(1, "anil", 78),(2,"bhumika",93),(3,"chetan",85),(4, "dhruv", 96),
(5,"emanual", 92),(6, "farah",82);
SELECT * FROM stud;
#Q.GET names of all students who scored more than class average
-- find average of class 
-- names  with mark s > avevrage
SELECT AVG(marks)
FROM stud;

SELECT name ,marks
FROM stud
WHERE marks > (SELECT AVG(marks)
FROM stud);   # this process makes it automatic

#Q: find name all students whose roll no is even?
-- sterp 1:find even roll no
-- step 2 names of even roll nos
SELECT name, rollno 
FROM STUD 
WHERE rollno % 2 = 0
ORDER BY rollno;

select * from stud where rollno %2= 0;
select name, marks from stud where rollno in(2,4,6);
select name, marks from stud where rollno % 2=0;

SELECT name, rollno
FROM STUD
WHERE rollno IN (
    SELECT rollno
    FROM STUD
    WHERE rollno % 2 = 0
)
ORDER BY rollno;


SELECT name, rollno
FROM stud
WHERE rollno IN(
	SELECT rollno
    FROM stud
    WHERE rollno % 2 = 0);
ALTER TABLE student
ADD COLUMN city VARCHAR(50);
insert into stud(name, city) values ("anil","pune"),("bhumika","mumbai"),
("chetan", "mumbai"), ("dhruv","delhi"), ("emanual", "delhi"), ("farah","delhi");

CREATE TABLE stud2 (
rollno INT (1) PRIMARY KEY,
name VARCHAR(50),
marks INT (3),
city VARCHAR (50) 
);
INSERT INTO stud2 VALUES 
(1, "anil", 78,"pune"),(2,"bhumika",93,"mumbai"),(3,"chetan",85,"mumbai"),(4,"dhruv", 96,"delhi"),
(5,"emanual", 92,"delhi"),(6, "farah",82,"delhi");
SELECT * FROM stud2;
-- Q find max marks from student of delhi with use of from
-- step 1 find student of delhi
-- step 2 find their max marks using sublist in step 1
select *
from stud2
where city = "delhi";
# we can use only this table
select max(marks)
from (select *
	from stud2
	where city = "delhi") as temp; # this gives maximum marks 96
    

select max(marks) 
from stud2
where city= "delhi";

select (select max(marks) from stud2), name
from stud2;   # not so useful query
create view view1 as   # only some part of full table can be seen snd operated
select rollno, name, marks from stud2;
select * from view1
where marks > 90;

drop view view1;
select * from view1;   # as we deleted view
