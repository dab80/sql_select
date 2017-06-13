```sql
mysql> SELECT first_name,
    ->        last_name
    -> FROM student;
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| Eric       | Ephram    |
| Greg       | Gould     |
| Adam       | Ant       |
| Howard     | Hess      |
| Charles    | Caldwell  |
| James      | Joyce     |
| Doug       | Dumas     |
| Kevin      | Kraft     |
| Frank      | Fountain  |
| Brian      | Biggs     |
+------------+-----------+
10 rows in set (0.00 sec)

mysql> SELECT *
    -> FROM student
    -> WHERE years_of_experience < 8;
+------------+------------+-----------+---------------------+------------+
| student_id | first_name | last_name | years_of_experience | start_date |
+------------+------------+-----------+---------------------+------------+
|          1 | Eric       | Ephram    |                   2 | 2016-03-31 |
|          2 | Greg       | Gould     |                   6 | 2016-09-30 |
|          3 | Adam       | Ant       |                   5 | 2016-06-02 |
|          4 | Howard     | Hess      |                   1 | 2016-02-28 |
|          5 | Charles    | Caldwell  |                   7 | 2016-05-07 |
|          8 | Kevin      | Kraft     |                   3 | 2016-04-15 |
|         10 | Brian      | Biggs     |                   4 | 2015-12-25 |
+------------+------------+-----------+---------------------+------------+
7 rows in set (0.00 sec)

mysql> SELECT last_name,
    ->        start_date,
    ->        student_id
    -> FROM student
    -> ORDER BY last_name;
+-----------+------------+------------+
| last_name | start_date | student_id |
+-----------+------------+------------+
| Ant       | 2016-06-02 |          3 |
| Biggs     | 2015-12-25 |         10 |
| Caldwell  | 2016-05-07 |          5 |
| Dumas     | 2016-07-04 |          7 |
| Ephram    | 2016-03-31 |          1 |
| Fountain  | 2016-01-31 |          9 |
| Gould     | 2016-09-30 |          2 |
| Hess      | 2016-02-28 |          4 |
| Joyce     | 2016-08-27 |          6 |
| Kraft     | 2016-04-15 |          8 |
+-----------+------------+------------+
10 rows in set (0.00 sec)

mysql> SELECT *
    -> FROM student
    -> WHERE first_name IN ('Adam',
    ->                      'James',
    ->                      'Frank')
    -> ORDER BY last_name DESC;
+------------+------------+-----------+---------------------+------------+
| student_id | first_name | last_name | years_of_experience | start_date |
+------------+------------+-----------+---------------------+------------+
|          6 | James      | Joyce     |                   9 | 2016-08-27 |
|          9 | Frank      | Fountain  |                   8 | 2016-01-31 |
|          3 | Adam       | Ant       |                   5 | 2016-06-02 |
+------------+------------+-----------+---------------------+------------+
3 rows in set (0.00 sec)

mysql> SELECT first_name,
    ->        last_name,
    ->        start_date
    -> FROM student
    -> WHERE start_date BETWEEN '2016-01-01' AND '2016-06-30'
    -> ORDER BY start_date DESC;
+------------+-----------+------------+
| first_name | last_name | start_date |
+------------+-----------+------------+
| Adam       | Ant       | 2016-06-02 |
| Charles    | Caldwell  | 2016-05-07 |
| Kevin      | Kraft     | 2016-04-15 |
| Eric       | Ephram    | 2016-03-31 |
| Howard     | Hess      | 2016-02-28 |
| Frank      | Fountain  | 2016-01-31 |
+------------+-----------+------------+
6 rows in set (0.00 sec)

#############################################################
                       MEDIUM CHALLENGE
#############################################################
mysql> CREATE TABLE `assignment` (
    ->   `assignment_id` int(11) unsigned NOT NULL AUTO_INCREMENT,
    ->   `student_id` int(11) NOT NULL,
    ->   `assignment_nbr` int(11) NOT NULL,
    ->   `grade` varchar(30) DEFAULT NULL,
    ->   `class_id` int(11) DEFAULT NULL,
    ->   PRIMARY KEY (`assignment_id`)
    -> ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
Query OK, 0 rows affected (0.07 sec)

CREATE TABLE `grade` (
  `grade_id` TINYINT(2) unsigned NOT NULL AUTO_INCREMENT,
  `grade` varchar(28) NOT NULL,
   PRIMARY KEY (`grade_id`)
 ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

ALTER TABLE ASSIGNMENT ADD COLUMN grade_id tinyint(2) unsigned;

CREATE INDEX idx_grade_id ON ASSIGNMENT (grade_id);

ALTER TABLE `assignment` ADD CONSTRAINT fk_grade_id
FOREIGN KEY (grade_id) REFERENCES grade(grade_id);

mysql> show tables;
+---------------+
| Tables_in_tiy |
+---------------+
| assignment    |
| student       |
+---------------+
2 rows in set (0.00 sec)

mysql> select * from assignment;
Empty set (0.00 sec)

mysql> explain assignment;
+----------------+------------------+------+-----+---------+----------------+
| Field          | Type             | Null | Key | Default | Extra          |
+----------------+------------------+------+-----+---------+----------------+
| assignment_id  | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
| student_id     | int(11)          | NO   |     | NULL    |                |
| assignment_nbr | int(11)          | NO   |     | NULL    |                |
| grade          | varchar(30)      | YES  |     | NULL    |                |
| class_id       | int(11)          | YES  |     | NULL    |                |
+----------------+------------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)

mysql> alter table assignment drop column grade;
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> explain assignment;
+----------------+------------------+------+-----+---------+----------------+
| Field          | Type             | Null | Key | Default | Extra          |
+----------------+------------------+------+-----+---------+----------------+
| assignment_id  | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
| student_id     | int(11)          | NO   |     | NULL    |                |
| assignment_nbr | int(11)          | NO   |     | NULL    |                |
| class_id       | int(11)          | YES  |     | NULL    |                |
+----------------+------------------+------+-----+---------+----------------+
4 rows in set (0.00 sec)

mysql> alter table assignment add column grade_id int;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> explain assignment;
+----------------+------------------+------+-----+---------+----------------+
| Field          | Type             | Null | Key | Default | Extra          |
+----------------+------------------+------+-----+---------+----------------+
| assignment_id  | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
| student_id     | int(11)          | NO   |     | NULL    |                |
| assignment_nbr | int(11)          | NO   |     | NULL    |                |
| class_id       | int(11)          | YES  |     | NULL    |                |
| grade_id       | int(11)          | YES  |     | NULL    |                |
+----------------+------------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)


#############################################################
                        HARD CHALLENGE
#############################################################

CREATE INDEX idx_student_id ON ASSIGNMENT (student_id);

ALTER TABLE `assignment` ADD CONSTRAINT fk_student_id
FOREIGN KEY (student_id) REFERENCES student(student_id);

mysql> explain student;
+---------------------+------------------+------+-----+---------+----------------+
| Field               | Type             | Null | Key | Default | Extra          |
+---------------------+------------------+------+-----+---------+----------------+
| student_id          | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
| first_name          | varchar(30)      | YES  |     | NULL    |                |
| last_name           | varchar(30)      | YES  |     | NULL    |                |
| years_of_experience | tinyint(3)       | YES  |     | NULL    |                |
| start_date          | date             | YES  |     | NULL    |                |
+---------------------+------------------+------+-----+---------+----------------+
5 rows in set (0.03 sec)

mysql> explain assignment;
+----------------+---------------------+------+-----+---------+----------------+
| Field          | Type                | Null | Key | Default | Extra          |
+----------------+---------------------+------+-----+---------+----------------+
| assignment_id  | int(11) unsigned    | NO   | PRI | NULL    | auto_increment |
| student_id     | int(11) unsigned    | NO   | MUL | NULL    |                |
| assignment_nbr | int(11)             | NO   |     | NULL    |                |
| class_id       | int(11)             | YES  |     | NULL    |                |
| grade_id       | tinyint(2) unsigned | YES  | MUL | NULL    |                |
+----------------+---------------------+------+-----+---------+----------------+
5 rows in set (0.01 sec)

mysql> explain grade;
+----------+---------------------+------+-----+---------+----------------+
| Field    | Type                | Null | Key | Default | Extra          |
+----------+---------------------+------+-----+---------+----------------+
| grade_id | tinyint(2) unsigned | NO   | PRI | NULL    | auto_increment |
| grade    | varchar(28)         | NO   |     | NULL    |                |
+----------+---------------------+------+-----+---------+----------------+
2 rows in set (0.02 sec)

mysql> INSERT INTO ASSIGNMENT (assignment_nbr,
    ->                         class_id,
    ->                         grade_id)
    -> VALUES (127,
    ->         2017,
    ->         3);
ERROR 1364 (HY000): Field 'student_id' doesn't have a default value
```
