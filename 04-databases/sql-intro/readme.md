# PostgreSQL
Postgres is the DB system we will be working with.

If you installed Postgres.app you have access to psql from the elephant icon at the top of the screen:

* ![image](Postgres.png)

[Postgres Installation: OSX](/00-config-deployment/installfest/osx/readme.html#postgres)

---

#### Postgres.app default settings

|               |                               |
| ------------- |:-------------:                |
| host          | localhost                     |
| port          | 5432                          |
| user          | __your default system user__  |
| Database      | __same as username__          |
| password      | none                          |
| connection URL| postgresql://localhost        |

---

## PSQL
If you are using the command line:

* In your terminal, type `psql`.

**psql** is a command line tool to interact with postgres databases, by default it connects to the localhost database with the name of the current user

psql has some of it's own command which begin with `\`

List all of the available databases:

```
\list
```

---

List all of the available tables in the current database:

```
\dt
```

Check your connection information:

```
\conninfo
```

There are lots of other commands which you can see with:

```
\?
```
Use `\q` to exit the help screen

Note that all psql commands start with `\`

---

## SQL: Structured Query Language

## Creating a Database

Most database products have the notion of separate databases.  Lets create one for the lesson.
```
CREATE DATABASE testdb;
```

**ALL SQL COMMANDS MUST BE ENDED WITH A SEMICOLON IN THE PSQL SHELL**

Connect to a database
```
\connect testdb
```

Once we connect, our command prompt should look similar to this: `testdb=#`

List our tables in a database:
```
\dt
```

---

##Database Schema Design

For this lesson, how to design a complete database system is out of scope. We will discuss a few things here though.

The **schema** of the database is the set of create table commands that specify what the tables are and how they relate to each other. For our very simple database example, here is the schema:

```
testdb=# \d+ students
                                                    Table "public.students"
 Column |         Type          |                       Modifiers
--------+-----------------------+-------------------------------------------------------
 id     | integer               | not null default nextval('students_id_seq'::regclass)
 name   | text                  |
 phone  | character varying(15) |
 email  | text                  |

```

---

## What is a Primary Key?

It denotes an attribute on a table that can uniquely identify the row.

### What does SERIAL Do?

SERIAL tells the database to automatically assign the next unused integer value to id whenever we insert into the database and do not specify id. In general, if you have a column that is set to SERIAL, it is a good idea to let the database assign the value for you.

---

## Data Types

Similar to how javascript has types of data, SQL defines types that can be stored in the DB. Here are some common ones:

* Serial
* Integer
* Numeric // Numbers are exact, no rounding error
* Float // Rounding error is possible, but operations are faster than Numeric
* Text, Varchar
* Timestamp
* Boolean (True or False)

You can set the storage size of some of these fields, but it could be a premature optimization that can cause problems later- you can't change the type while the DB is running.

---

### Data Types in Practice
- integer (sometimes numeric for non-integers)
- boolean
- timestamp
- text for everything else

---

### CREATE-ing a Table

This is an example of a students table.  (We will talk about the primary key soon.)
```sql
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name TEXT,
    phone VARCHAR(15),
    email TEXT
);

```

Check that it's there:
```
\dt
```

Look at the table structure
```
 \d+ students
```

---

### INSERT-ing Data

```sql
INSERT INTO students
(name, phone, email)
VALUES
('William Smith', '(415)555-5555', 'bill@example.com');

INSERT INTO students
(name, phone, email)
VALUES
('Bob Jones', '(415)555-5555', 'bob@example.com');

```

---

### SELECT-ing Data

```sql
SELECT * FROM students;

SELECT * FROM students WHERE name = 'Bob Jones';

SELECT id, name FROM students;
```

---

## UPDATE-ing Data

```sql
UPDATE students SET email='bobby@example.com' WHERE name = 'Bob Jones';
```

---

## DELETE-ing Data

```sql
DELETE from students WHERE name = 'Mary';
->DELETE 0
```

```sql
DELETE from students WHERE email = 'bobby@example.com';
->DELETE 1
```
Note: in pratice you will hardly ever delete anything, but mostly set a boolean 'valid' or something similar.

---

### DROP-ing a Table

```sql
DROP TABLE students;
```

---

### The WHERE clause
- can contain basically any conditional statement
- comparison ... > < <= >=
- and logical operators- AND OR NOT etc

---

### The ORDER BY clause
```
ORDER BY column_name, column_name ASC
```

ASC (ascending)
DESC (descending)

---


### Pairing Exercise

Design a table for a movie database.

It will have a `title`, `description` and `rating`. `title` and `description` will be text, and rating will be an integer.

Make the CREATE TABLE command and execute it in psql.

Use \dt to verify that the table was created.

Once you're satisfied that the table is there, get rid of it using the DROP TABLE command. Use \dt again to make sure that the table has been dropped.

## Selecting

A select statement allows you to get data from the database. Postgres has a good [tutorial on select](http://www.postgresql.org/docs/7.3/static/tutorial-select.html). I'd recommend looking at the tutorial sometime after the lesson.

Create a new database to hold a movies table:

```sql
CREATE DATABASE testdb;
```

Connect to the new database:
```sql
\connect testdb;
```

Given this table:

```sql
CREATE TABLE movies (
  id SERIAL PRIMARY KEY,
  title TEXT,
  description TEXT,
  rating INTEGER
);
```

And these insert statements:

```sql
INSERT INTO movies (title, description, rating) VALUES('Cars', 'a movie', 9);
INSERT INTO movies (title, description, rating) VALUES('Back to the Future', 'another movie', 9);
INSERT INTO movies (title, description, rating) VALUES('Dude Wheres My Car', 'probably a bad movie', 3);
INSERT INTO movies (title, description, rating) VALUES('Godfather', 'good movie', 10);
INSERT INTO movies (title, description, rating) VALUES('Mystic River', 'did not see it', 7);
INSERT INTO movies (title, description, rating) VALUES('Argo', 'a movie', 8);
INSERT INTO movies (title, description, rating) VALUES('Gigli', 'really bad movie', 1);
```

This will select all the attributes from the movies table unconditionally.  Make sure not to forget the ; at the end of each statement. In SQL, semi colons are required to terminate statements.

```sql
SELECT * FROM movies;
````

We may not want all attribues though.  Let's say instead we only care about the titles of the movie and the description.  Here is the query we'd use:

```sql
SELECT title, description FROM movies;
```


This will select the titles from movies table where the rating is greater than 4.

```sql
SELECT title FROM movies WHERE rating > 4;
```

You can also have more complex queries to get data.  The following query finds all the movies with a rating greater than 4 and with a title of Cars.

```sql
SELECT title FROM movies WHERE rating > 4 AND title = 'Cars';
```

SQL also supports an OR statement.  The following query will return any movie with a rating greater than 4, or any movies with the title Gigli.  In other words, if a movie called Gigli is found with a rating equal to 1, it will still be returned along with any movie with a rating greater than 4.

```sql
SELECT title FROM movies WHERE rating > 4 OR title = 'Gigli';
```

Let's say that we just want a list of the best movies of all time.  We can do a select statement that ensures ordering.  The DESC keyword tells it to order the rating in descending order. ASC is the default.

```sql
SELECT title, rating FROM movies ORDER BY rating DESC;
```

**Note:** If no order by clause is specified, the database does not give any guarantees on what order your data will be returned in. At times it may seem like data you are getting back is in sorted order, but make sure not to rely on that in your code. Only rely on a sort if you also have an ORDER BY clause.

We've gotten a list of movies back, but it's way too long for our uses.  Let's instead only get the top 5 movies that are returned using LIMIT:

```sql
SELECT title, rating FROM movies ORDER BY rating DESC LIMIT 5;
```

### Pairing Exercise

Write a query on the movie table to return the worst movie of all time.  There should be only 1 result returned. The result should include the title, description and rating of the movie.

## Updating

The update statement is defined [here](http://www.postgresql.org/docs/9.1/static/sql-update.html) in the postgres docs. It is used to change existing data in our database.

For example, if we do not think Gigli was actually that bad, and we want to change the rating to a 2, we can use an update statement:

```sql
UPDATE movies SET rating=2 WHERE title='Gigli';
```

## Deleting

Deleting works similarly to a select statement. Here are the [docs on delete](http://www.postgresql.org/docs/8.1/static/sql-delete.html)

The statement below deletes the Dude Wheres My Car row from the database:

```sql
DELETE FROM movies WHERE title='Dude Wheres My Car';
```

We could also use compound statements here:

```sql
DELETE FROM movies WHERE id < 9 AND rating = 2;
```
