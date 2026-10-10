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
## Clauses 

A clause is a part of a statement that specifies the criteria of the data being manipulated, usually by an initial statement. Clauses can help us define the type of data and how it should be retrieved or sorted. 

In previous tasks, we already used some clauses, such as `FROM` that is used to specify the table we are accessing with our statement and `WHERE`, which specifies which records should be used.  

We will focus on other clauses: `DISTINCT`, `GROUP BY`, `ORDER BY`, and `HAVING`.

### DISTINCT Clause

The `DISTINCT` clause is used to avoid duplicate records when doing a query, returning only unique values.

Let's use a query `SELECT * FROM books` and observe the results below.

![[Pasted image 20261008150501.png]]

The query's output displays all the content of the table **books**, and the record **Ethical Hacking** is displayed twice. Let's perform the query again, but this time, using the `DISTINCT` clause.

![[Pasted image 20261008150519.png]]

The output shows that only five rows are returned, and just one instance of the **Ethical Hacking** record is displayed.

### GROUP BY Clause

The `GROUP BY` clause aggregates data from multiple records and **groups** the query results in columns. This can be helpful for aggregating functions.

![[Pasted image 20261008151120.png]]

In the example above, the records on the **book** table are regrouped by the result of the `COUNT` function. We already know that **Ethical hacking** is listed twice, so the total **count** is 2, placed at the end since it is **grouped** **by** count.

### ORDER BY Clause

The `ORDER BY` clause can be used to sort the records returned by a query in ascending or descending order. Using functions like `ASC` and `DESC` can help us to accomplish that, as shown below in the next two examples.

**ASCENDING ORDER**

![[Pasted image 20261008151247.png]]

**DESCENDING ORDER**

![[Pasted image 20261008152646.png]]

We can observe the difference when sorting by ascending order using `ASC` and in descending order using `DESC`, both using the **published_date** as reference.

### HAVING Clause

The `HAVING` clause is used with other clauses to filter groups or results of records based on a condition. In the case of `GROUP BY`, it evaluates the condition to `TRUE` or `FALSE`, unlike the `WHERE` clause `HAVING` filters the results after the aggregation is performed.

![[Pasted image 20261008153810.png]]

In the example above, we can observe that the query returns the books with the names that contain the word **hack** and the proper count, as we learned before.

----
## Operators 

### Logical Operators 

These operators test the truth of a condition and return a boolean value of `TRUE` or `FALSE`. 
### Like Operator

The `LIKE` operator is commonly used in conjunction with clauses like `WHERE` in order to filter for specific patterns within a column.

![[Pasted image 20261008154803.png]]

The query above returns a list of records from the books filtered, but the ones using the `WHERE` clause that contains the word guide by using the `LIKE` operator.

### AND Operator

The `AND` operator uses multiple conditions within a query and returns `TRUE` if all of them are true.

![[Pasted image 20261008154845.png]]

The query above returns the book with the name **Bug Bounty Bootcamp**, which is under the category of **Offensive Security**.

### OR Operator

The `OR` operator combines multiple conditions within queries and returns `TRUE` if at least one of these conditions is true.

![[Pasted image 20261008155039.png]]

The query above returns books whose **names** include either **Android** or **IOS**.

### NOT Operator

The `NOT` operator reverses the value of a boolean operator, allowing us to exclude a specific condition.

![[Pasted image 20261008155154.png]]

The query above returns results where the description does not contain the word **guide**.


### BETWEEN Operator

The `BETWEEN` operator allows us to test if a value exists within a defined **range**.

![[Pasted image 20261008155221.png]]

The query above returns books whose **id** is **between 2** and **4**.

### Comparison Operators

The comparison operators are used to compare values and check if they meet specified criteria.

### Equal To Operator

The `=` (Equal) operator compares two expressions and determines if they are equal, or it can check if a value matches another one in a specific column.

![[Pasted image 20261008155306.png]]

The query above returns the book with the **exact name Designing Secure Software**.


### Not Equal To Operator

The `!=` (not equal) operator compares expressions and tests if they are not equal; it also checks if a value differs from the one within a column.

![[Pasted image 20261008155338.png]]

The query above returns books **except** those whose **category** is **Offensive Security**.

### Less Than Operator

Less Than Operator

The `<` (less than) operator compares if the expression with a given value is lesser than the provided one.

![[Pasted image 20261008155407.png]]

The query above returns books that were published **before January 1, 2020**.

### Greater Than Operator

The `>` (greater than) operator compares if the expression with a given value is greater than the provided one.

![[Pasted image 20261008155437.png]]

The query above returns books published **after** **January 1, 2020**.

### Less Than or Equal To and Greater Than or Equal To Operators


The `<=` (Less than or equal) operator compares if the expression with a given value is less than or equal to the provided one. On the other hand, The `>=` (Greater than or Equal) operator compares if the expression with a given value is greater than or equal to the provided one. Let's observe some examples of both below.

![[Pasted image 20261008155521.png]]

The query above returns books **published on** **or before** **November 15, 2021**.

![[Pasted image 20261008155537.png]]

The query above returns books that were **published on or after November 2, 2021**.

---
## Functions 

### String Function

Strings functions perform operations on a string, returning a value associated with it.

### CONCAT() Function

This function is used to add two or more strings together. It is useful to combine text from different columns.


![[Pasted image 20261008164949.png]]

This query concatenates the **name** and **category** columns from the **books** table into a single one named **book_info**.

### GROUP_CONCAT() Function

This function can help us to concatenate data from multiple rows into one field. Let's explore an example of its usage.

![[Pasted image 20261008165114.png]]

The query above groups the **books** by **category** and concatenates the titles of books within each category into a **single string**.

### SUBSTRING() Function

This function will retrieve a substring from a string within a query, starting at a determined position. The length of this substring can also be specified.

![[Pasted image 20261008165254.png]]

In the query above, we can observe how it extracts the first **four** characters from the **published_date** column and stores them in the **published_year** column.

### LENGTH() Function

This function returns the number of characters in a string. This includes spaces and punctuation. We can find an example below.

![[Pasted image 20261008165423.png]]

As we can observe above, the query calculates the length of the string within the **name** column and stores it in a column named **name_length**.

**Aggregate Functions**

These functions aggregate the value of multiple rows within one specified criteria in the query; It can combine multiple values into one result.

### COUNT() Function

This function returns the number of records within an expression, as the example below shows.

![[Pasted image 20261008165517.png]]

This query above counts the total number of rows in the **books** table. The result is **5**, as there are five books in the books table, and it's stored in the **total_books** column.

### SUM() Function

This function sums all values (not NULL) of a determined column.

**Note:** There is no need to execute this query. This is just for example purposes.

![[Pasted image 20261008165551.png]]

The query above calculates the total sum of the **price** column. The result provides the aggregate price of all books in the column **total_price**.

### MAX() Function

This function calculates the maximum value within a provided column in an expression.

![[Pasted image 20261008165625.png]]

The query above retrieves the latest publication (maximum value) date from the **books** table. The result **2021-12-21** is stored in the column **latest_book**.

### MIN() Function

This function calculates the minimum value within a provided column in an expression.

![[Pasted image 20261008165655.png]]

The query above retrieves the earliest publication (minimum value) date from the **books** table. The result **2014-10-14** is stored in the **earliest_book** column.


---
## Related 

[[MOC_Development|Development]]


