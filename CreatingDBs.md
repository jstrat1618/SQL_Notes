# Datatypes
PostgreSQL supports the following data types
-Boolean 
-Character
-Number
-Temporal
-Special types (like geometric)
-Array

## Booleans
Booleans are either true or false.
Keyword: bool (more on keywords later)
If you insert a 1, yes, y, t or true they are all converted to true; whereas 0, no, and f are all converted to false

## Character
There are three types of character data types which vary regarding length
* A single character: char
* Fixed-length character: char(n)- these variables MUST BE of length n
* Varaiable-length: varchar(n)- these must be shorter than n
PostgreSQL doesn't pad spaces

## Numbers
Two major types of Number data types: Integers and floating-points
## Integers
3 types of integers
* Small Integer (2 byte signed integer has a range from -32768,32767
* Integer- 4 byte integer that has a much larger
* Serial- Is the same as intger but it populates automatically

## Floating point number
* float(n) is a floating point number less than n, up to a max of 8 bytes
* real or float8 
* numeric or numeric(p,s)- a real number with p digits and s numbers after the decimal point. The numeric(p,) is the exact number

## Temporal
- date stores date data
-time stores time data
-timestamp stores date and time
-interval stores the difference in times
- timestamptz stores date, time and time zone


# Primary Keys and Foreign Keys

Definition of Primary Key: A primary key is a column or group of columns that is used to identify a row uniquely in a table.

* You define primary keys through primary key constraints (More on that later)

A table can have one and only one primary key.
It is good practice to add a primary key to every table. 
Primary keys are often of the serial data type because serial data types auto-increment.

Normally, we add a primary key to a table when we create the table's structure using the CREATE TABLE statement.

## Basic syntax
CREATE TABLE table_name(
col1_name data_type PRIMARY KEY,
col2_name data_type,
col3_name data_type, ...
);

# Foreign Keys
Definition of Foreign Key: A foreign key is a field or group of fields in a table that uniquely identifies a row in another table.

In other words, a foreign key is defined in a table that reference a primary key of the other table.

A table that contains the foriegn key is called the "referencing table" or "child table", and the table to which the foreign key references is called the "referenced table" or "parent table".

A table can have multiple foreign keys depending on its relationships with other tables.

In PosgreSQL you define a foreign key through a foreign key contraint.

A foreign key contraint indicates that values in a column or group of columns in the child table match the values in a column or group of columns of a parent table.

We say that a foreign key contraint maintains referential integrity between child and parent tables.

# CREATE TABLE
To create a table in PostgreSQL you use the CREATE TABLE statement

## Basic syntax
CREATE TABLE table_name
(col1_name TYPE col_constraint
table_constraints)
INHERITS exisiting_table_name;

When a table inherits from an exisiting table it means the new table contains all columns of the existing tables and the columns defined in the CREATE TABLE statement.

# PostgreSQL column constraints
* NOT NULL- the value of the column cannot be NULL.
* UNIQUE- the value of the column must be unique across the whole table. However, the column can have many NULL values because PostgreSQL treats each NULL value to be unique.
NOTE: that SQL standard only allows one NULL value in the column that has the UNIQUE constraint
* PRIMARY KEY- this constraint is the combination of NOT NULL and UNIQUE constraints
NOTE: You can define multiple columns as the primary key by using the PRIMARY KEY constraint as a table-level constraint.
* CHECK- enables to check a condition when you insert or update data (for example data is positive)
* REFERENCES- constrains the value of the column that exists in a column in another table

# PostgreSQL constraints
Table constraints are very similar to column constraints except they apply to more than one column.

* UNIQUE (column_list)- force the value stored in the columns listed inside the parentheses to be unique.
* PRIMARY KEY (column_list)- to define the primary key that consists of multiple columns
* CHECK (condition)- to check a condition when inserting or updating data
* REFERENCES- to constrain the value stored in the column that must exist in a column in another table.

# Example
In this example we will create 3 tables with the following tables:
Role
role_id: int4 primary key
role_name: varchar(255)

Account_Role
user_id: int4 primary key
role_id: int4 foreign key referencing role_id int he Role table
grand_date: timestamp

Account
user_id: int4 foreign key referencing the user_id in the Account_Role table.
username: varchar(50)
password: varchar(50)
email: varchar(355)
created_on: timestamp
last_login: timestamp

## Creating account table
CREATE TABLE account(
	user_id serial PRIMARY KEY,
	username varchar(50) UNIQUE NOT NULL,
    password varchar(50) NOT NULL,
    email varchar(355) UNIQUE NOT NULL,
    create_on timestamp NOT NULL,
    last_login timestamp    
);

## Creating role table
CREATE TABLE role(
role_id serial PRIMARY KEY,
role_name varchar(25) UNIQUE NOT NULL
);

## Creating account_role
CREATE TABLE account_role
(
  user_id integer NOT NULL,
  role_id integer NOT NULL,
  grant_date timestamp without time zone,
  PRIMARY KEY (user_id, role_id),
  CONSTRAINT account_role_role_id_fkey FOREIGN KEY (role_id)
      REFERENCES role (role_id) MATCH SIMPLE
      ON UPDATE NO ACTION ON DELETE NO ACTION,

  CONSTRAINT account_role_user_id_fkey FOREIGN KEY (user_id)
      REFERENCES account (user_id) MATCH SIMPLE
      ON UPDATE NO ACTION ON DELETE NO ACTION
);

# Challenge
Create a table to organize our potential leads! We will have the following information:

A customer's first name, last name,email,sign-up date, and number of minutes spent on the dvd rental site. You should also have some sort of id tracker for them. You have free reign on how you want to create this table, the next lecture will show one possible implementation of this.

## Solution
There's a lot of different ways to do this but here is one way

CREATE TABLE leads(
lead_id serial PRIMARY KEY,
first_name varchar(50) NOT NULL,
last_name varchar(50) NOT NULL,
email varchar(350) UNIQUE NOT NULL,
sign_up_date timestamp NOT NULL,
minutes_spent integer NOT NULL
);

# INSERT
The INSERT command allows you to insert one or more rows into a table.

## Basic syntax for manual insertion
### For one row insertion
INSERT INTO table_name(col1, col2, ...)
VALUES (value1, value2, ...)

### For more than one rows
INSERT INTO table_name(col1, col2, ...)
VALUES (value1, value2, ...)
       (value1, value2, ...), ...;

## Basic syntax for insertion from another table
INSERT INTO table_name
SELECT col1, col2, ...
FROM another_table_name
WHERE condition;

# Example
CREATE TABLE link(
ID serial PRIMARY KEY,
url VARCHAR(255) NOT NULL,
name VARCHAR(255) NOT NULL,
description VARCHAR(255),
rel VARCHAR(50));

## Insert a single row into link
INSERT INTO link(url, name)
	VALUES
	('www.google.com', 'google');

Let's insert another row

INSERT INTO link(url, name)
	VALUES
	('www.yahoo.com', 'Yahoo');

## Insert multiple rows into link
INSERT INTO link(url, name)
	VALUES
    ('www.bing.com', 'Bing'),
	('www.amazon.com', 'Amazon');

# Example of inserting data from another table
First, let's copy the link table. We can do this as follows

CREATE TABLE link_copy (LIKE link);

This copies the schema of the table.

Let's insert the "bing" row into link_copy
INSERT INTO link_copy
	SELECT * FROM link
	WHERE name = 'bing';

# Update Statement
To change the values of the columns in a table you use the UPDATE statement.

## Basic syntax
UPDATE table
SET col1 = val1,
    col2 = val2, ...
WHERE condition

# Example
UPDATE link 
SET description = 'Empty Description';	

UPDATE link
SET description = 'Starts with an A';
WHERE name LIKE 'A%';

You can return columns after an update usig the RETURNING statement;

## Example
UPDATE link 
SET description = 'New Description'
WHERE id = 1
RETURNING id, url, name, description;

# The DELETE statement
The DELETE statement is used to delete rows.

The delete statement returns the number of rows deleted.


## Basic syntax
DELETE FROM table
WHERE condition


## Example
Let's delete all the rows in the link table where name starts with a "B".

You can also use the RETURNING statement to verify which rows were deleted.

DELETE FROM link
WHERE name = 'A'
RETURNING *;

# ALTER TABLE
## Basic syntax
ALTER TABLE table_name action;

There are several different action statements. Here are some of the things you can do with action statements.
* Add, remove or rename column
* Set default for the column.
* Add CHECK constraint to a column
* Rename table

Some of the keywords we will use will be
* ADD COLUMN
* DROP COLUMN
* RENAME COLUMN
* ADD CONSTrAINT
* RENAME TO

## Examples
Here we will drop the link table if it exists.

DROP TABLE IF EXISTS link;

Now, we will recreate a table called link with 3 columns link_id, title, and url. Here's the code to do that.

CREATE TABLE link(
link_id serial PRIMARY KEY,
title VARCHAR(500) NOT NULL,
url VARCHAR(1000) NOT NUL UNIQUE);

Let's add a column called "active" to indicate whether the link is active or not (a boolean).

ALTER TABLE link
ADD COLUMN active boolean; 

Say we changed our minds and we want to drop the column "active"

ALTER TABLE link
DROP COLUMN active;


Let's say we want to rename the "title" column

ALTER TABLE link
RENAME COLUMN title TO new_title_name;

We can also rename the entire table.

ALTER TABLE link
RENAME TO url_table;

# The DROP TABLE
You can use DROP TABLE to drop a table from your database. 

## Basic syntax
DROP TABLE [IF EXISTS] table_name;

THe IF EXISTS is in side brackets because it is optional.

## EXAMPLE
CREATE TABLE test123(
test_id serial PRIMARY KEY);

Now, to drop the table

DROP TABLE test123;

If you run  "DROP TABLE test123;" again you will get an error, but if you run

DROP TABLE IF EXISTS test123;

You will get a notice that the table does not exist.

NOTE: There is another key word RESTRICT that follows a DROP TABLE statement that restricts the drop if another object is dependent on that table.

PostgreSQL uses RESTRICT by default. If you wanted to drop it along with its dependencies you can use the keyword CASCADE.

# CHECK CONSTRAINT
A check Constraint is a kind of a constraint that allows you to specify if a value in a column meets a specific requirement.

The CHECK constraint uses a Boolean expression to evaluate the values of a column.

If the columns pass the check PostgreSQL will insert or update those values.

# Examples
CREATE TABLE new_users(
id serial PRIMARY KEY,
first_name VARCHAR(50),
bdate DATE CHECK(bdate > '1900-01-01'),
join_date DATE CHECK(join_date > bdate),
salary INTEGER CHECK(salary >= 0));

Try inserting a new row and we'll see if we get errors when we pass in data that violates the checks

We'll try to insert a row with a negative salaray and we should get an error.

INSERT INTO new_users (first_name, bdate, join_date, salary)
VALUES
('Joe', '1980-02-02', '1990-04-04', -10);


You can also name your constraints for example say we want to create a sales table with constraint that sales are positive and name that constraint "my_positive_sales_constraint" we would do this as follows.

CREATE TABLE checktest(
sale_id serial PRIMARY KEY,
sales INTEGER CONSTRAINT my_positive_sales_constraint CHECK(sales > 0));

Now, when we try insert a "-1" for sales with the following code

INSERT INTO checktest(sales)
VALUES
(-1);

we should get an error that says something like: 

ERROR:  new row for relation "checktest" violates check constraint "my_positive_sale_constraint"

# NOT NULL Constraint
* In database theory NULL is unknown or missinf information

* The NULL value is different from empty or zero
* For example, knowing that someone doesn't have an email address is different than not knowing the email address

## Example
CREATE TABLE learn_null(
first_name VARCHAR(50)
sales integer NOT NULL);

Let's insert someone's name but omit the sales values

INSERT INTO lear_null(first_name)
VALUES('John');

We should get an error as follows:
ERROR:  null value in column "sales" violates not-null constraint
DETAIL:  Failing row contains (John, null).
********** Error **********

But when we insert the sales for John as follows it should work

INSERT INTO learn_null(first_name, sales)
VALUES('John', 12)

# UNIQUE Constraint

The UNIQUE constrains allows you to ensure the uniqueness of data is maintained. For example, that no two users have the same email.

When the UNIQUE constraint is imposed everytime a new row is inserted PostgreSQL checks the uniqueness of that value. Same process is also carried out when you try to UPDATE a specific value.

## Example
CREATE TABLE people(
id SERIAL PRIMARY KEY,
first_name VARCHAR(50),
email VARCHAR(150) UNIQUE);

INSERT INTO people(id, first_name, email)
VALUES(1, 'Joe', 'joe@joe.com')

Now, let's see what happens when we try to insert someone with the same email

INSERT INTO people(id, first_name, email)
VALUES(2, 'Joseph', 'joe@joe.com')

We should get an error as follows

ERROR:  duplicate key value violates unique constraint "people_email_key"
DETAIL:  Key (email)=(joe@joe.com) already exists.
********** Error **********