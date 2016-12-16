# Databases

Storing large amounts of persistent data is an important aspect of nearly every full-stack web application. We can do this with files but mostly likely to scale, we'll use databases. Our applications will then have functionality to perform CRUD actions.

## What is CRUD?

CRUD is an acronym that stands for Create, Read, Update, Destroy. These are the basic operations that you can perform on data. Most websites you interact with on the internet are CRUD sites. Almost everything you do on the web is a CRUD action: Creating a user (create), Listing comments (read) to editing your profile (update), to deleting a video you uploaded on youtube (destroy).

## What is a database?

- It is a program that enforces structure on your data and allows a computer to quickly retreive data.
- A database should support CRUD operations

## Why Use a Database?
Discuss as a class. Why is it better than just writing to files?

* Data is structured
* Databases are transactional
* Data retrevial is fast
* Has a system for remote access
* Has a system for backup

## Types of Databases

**RDBMS** (Relational Database Management System) The most common type of database today is a **relational database**.  Relational databases have tabular data with rows for each instance of data and columns for each attribut of that data. Tables may refer to one another. Relational databases typically use **SQL** (Structured Query Language).

###Brands of Relational Databases

* Postgres
* MySQL
* Oracle (Commercial Product with lots of features)
* Microsoft SQL Server
* SQLite (Good for mobile development/Small applications)

**Cloud Storage**  This is a very vague term and can be used to mean lots of things. Typically it is a system in which your data is stored and managed by a company so you don't have to worry about losing it. Examples included AWS (Amazon Web Services), Rackspace, MS Azure

**NoSQL** There is also a school of thought called NoSql.  It is often a Key Value storage system and is not relational.  This is typically used in applications where a database does not scale well.  Example technologies include MongoDB, Apache CouchDB, SimpleDB.

### What we'll learn
We'll go over relational databases, which are the most widely used type of database. We'll be using SQL, which is the most popular query language for interacting with relational databases. We'll also cover MongoDB, which is a nonrelational database that relies on storing data in JSON-like documents.
