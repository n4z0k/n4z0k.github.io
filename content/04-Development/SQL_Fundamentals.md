---
author: Alfeze
created: 2026-10-08
---

# SQL Fundamentals

>  SQL (Structured Query Language) is a programming language that can be used to query, define and manipulate the data stored in a relational database.

---
## Databases

### Different Types of Databases

![[Pasted image 20261008002639.png|484]]

Two primary types : **relational databases** (aka SQL) and **non-relational databases** (aka NoSQL).

 - **Relational databases** are often used when the data being stored is reliably going to be received in a consistent format, where accuracy is important, such as when processing e-commerce transactions. 
 
 - **Non-relational databases**, on the other hand, are better used when the data being received can vary greatly in its format but need to be collected and organised in the same place, such as social media platforms collecting user-generated content.

### Tables, Rows and Columns

We’ll focus on relational databases. We’ll start by explaining **tables**, **rows**, and **columns**. All data stored in a relational database will be stored in a **table**; for example, a collection of books in stock at a bookstore might be stored in a table named “Books”.


![[Pasted image 20261008002652.png|473]]


### Primary and Foreign Keys

![[Pasted image 20261008002603.png]]


**Primary Keys**: A primary key is used to ensure that the data collected in a certain column is unique. That is, there needs to be a way to identify each record stored in a table, a value unique to that record and is not repeated by any other record in that table. Think about matriculation numbers in a university; these are numbers assigned to a student so they can be uniquely identified in records (as sometimes students can have the same name). A column has to be chosen in each table as a primary key; in our example, “id” would make the most sense as an id has been uniquely created for each book where, as books can have the same publication date or (in rarer cases) book title. Note that there can only be one primary key column in a table.

**Foreign Keys**: A foreign key is a column (or columns) in a table that also exists in another table within the database, and therefore provides a link between the two tables. In our example, think about adding an “author_id” field to our “Books” table; this would then act as a foreign key because the author_id in our Books table corresponds to the “id” column in the author table. Foreign keys are what allow the relationships between different tables in relational databases. Note that there can be more than one foreign key column in a table.

---
## SQL

>Databases are usually controlled using a Database Management System (DBMS). Serving as an interface between the end user and the database, a DBMS is a software program that allows users to retrieve, update and manage the data being stored. Some examples of DBMSs include MySQL, MongoDB, Oracle Database and MariaDB.

## Connecting to the DBMS

```shell
mysql -u root -p
```

## Database Statements

### CREATE DATABASE

```SQL
CREATE DATABASE database_name;
```

### SHOW DATABASES

```SQL
SHOW DATABASES;
```

Some databases that are included by default (`mysql`, `information_scheme`, `performance_scheme` and `sys`).

### USE DATABASE

```SQL
USE database_name;
```

### DROP DATABASE

```SQL
DROP DATABASE database_name;
```


---

## Table Statements

### CREATE TABLE

```SQL
CREATE TABLE example_table_name 
(example_column1 data_type, 
example_column2 data_type,
example_column3 data_type);
```

Example : 

```SQL
CREATE TABLE book_inventory (
book_id INT AUTO_INCREMENT PRIMARY KEY,
book_name VARCHAR(255) NOT NULL,
publication_date DATE
```

 This statement will create a table `book_inventory` with three columns: `book_id`, `book_name` and `publication_date`. `book_id` is an `INT` (Integer) as it should only ever be a number, `AUTO_INCREMENT` is present, meaning the first book inserted would be assigned book_id 1, the second book inserted would be assigned a book_id of 2, and so on. Finally, `book_id` is set as the `PRIMARY KEY` as it will be the way we uniquely identify a book record in our table (and a primary must be present in a table). 

 Book_name has the data type `VARCHAR(255)`, meaning it can use variable characters (text/numbers/punctuation) and a limit of 255 characters is set and `NOT NULL`, meaning it cannot be empty (so if someone tried to insert a record into this table but the book_name was empty it would be rejected. Publication_date is set as the data type `DATE`.

### SHOW TABLES

```SQL
SHOW TABLES;
```

### DESCRIBE

```SQL
DESCRIBE book_inventory;
```

### ALTER

```SQL
ALTER TABLE book_inventory ADD page_count INT;
```

### DROP TABLE

```SQL
DROP TABLE table_name;
```


---

## CRUD Operations

**CRUD** stands for **C**reate, **R**ead, **U**pdate, and **D**elete, which are considered the basic operations in any system that manages data.

### Create Operation (INSERT)

The **Create** operation will create new records in a table. In MySQL, this can be achieved by using the statement `INSERT INTO`, as shown below.

```SQL
INSERT INTO books (id, name, published_date, description)
VALUES (1, "Android Security Internals", "2014-10-14", "An In-Depth Guide to Android's Security Architecture");
```

### Read Operation (SELECT)

```SQL
SELECT * FROM books;
```

```SQL
SELECT name, description FROM books;
```

![[Pasted image 20261008014522.png]]


![[Pasted image 20261008014051.png]]

### Update Operation (UPDATE)

```SQL
UPDATE books SET description = "An In-Depth Guide to Android's Security Architecture." WHERE id = 1;
```

### Delete Operation (DELETE)

```SQL
DELETE FROM books WHERE id = 1;
```

### Summary

In summary, **CRUD** operations results are fundamental for data operations and when interacting with databases. The statements associated with them are listed below.

- **Create (INSERT statement)** - Adds a new record to the table.
- **Read (SELECT statement)** - Retrieves record from the table.
- **Update (UPDATE statement)** - Modifies existing data in the table.
- **Delete (DELETE statement)** - Removes record from the table.

These operations enable us to effectively manage and manipulate data within a database.


---

## Related 

[[MOC_Development|Development]]


