# Relational Tables in SQL

We know how to deal with one table, now we'll see how to deal with two tables that are related.

---

## One-to-many
One `Owner` has many `dogs` but one `dog` can only have one owner.

---


### How to relate one table to another: foreign keys
If we want one record to refer to another (one-to-many relationship) we can make a reference to that record by primary key id in the other table.

In the reffering table it's always named `tablename_id`

The dog table would have a column: `owner_id` that would contain the primary key of that row.

**Q. Why do we keep the relationship in the `dog` table?**

A: Each record/row can only have one value per column. (How would you store all of an owner's dog references in one column?)

---

### Implementing foreign keys:
tables.sql:
```
CREATE TABLE owners (
    id SERIAL PRIMARY KEY,
    name TEXT,
    phone TEXT,
    email TEXT
);

CREATE TABLE dogs (
    id SERIAL PRIMARY KEY,
    name TEXT,
    owner_id INTEGER
);
```

Get all of an owner's dogs:
Given you already know that owner is id=1;
```
SELECT * FROM dogs WHERE owner_id = 1;
```

#### Pairing Exercise
Use the examples above. Create a database that records dogs and users.

Use psql to make all of your sql queries that affect the DB.

Insert user records, then insert dogs that belong to the users.

##### Further
Create a database that records students and teachers.

##### Further
Create a node command line program that takes the name of the owner and prints out the dogs that belong to that owner.

(Note that this should require 2 different SQL queries)

Seed your DB:
```
INSERT INTO owners (name, phone, email) VALUES ('Chee Kean','34562876','ck@ga.co');
INSERT INTO owners (name, phone, email) VALUES ('Scott','37562876','scott@ga.co');

INSERT INTO dogs (name, owner_id) VALUES ('Fido', 1);
INSERT INTO dogs (name, owner_id) VALUES ('Susan Chan', 1);
INSERT INTO dogs (name, owner_id) VALUES ('Alice', 2);
INSERT INTO dogs (name, owner_id) VALUES ('Mr. Winkle Pants', 2);
```

When you run:
```
node index.js "Chee Kean"
```

Your program should return:
```
Fido, Susan Chan
```
