Why use databases?
-Data integrity
-Data size
-Combine datasets easier
-Automate steps for re-use
-Can support data for websites and applications

SQL (Structured Query Language) learned in this course can be applied to a variety of databses or SQL based software

# SQL
SQL is the programming language used to communicate to our database

### SQL example
SELECT customer_id, first_name, last_name
FROM sales
ORDER BY first_name;

Q&A's
https://www.udemy.com/the-complete-sql-bootcamp/learn/v4/t/lecture/4879170?start=0

# pgAdmin
pgAdmin is the most popular GUI for PostgreSQL

# Creating a Database
To create a database using SQL you can run the following SQL command where dbname is the name of the database.
CREATE DATABASE dbname; 
In pgAdmin you can also right click on database and go to 
-> Create -> Database
If you have the database as a .tar or .zip, then refresh Databases in pgAdmin, then go to the name of the database you just created, right click and go to Restore, then browse or paste the path to the full file, then hit restore.


# Restoring a database table schema only
* A very common task because you may want to copy a database with all the schema but you don't want to carry over the data perhaps because you fill that data with new data or you have privacy or proprietary concerns about the data

### Creating a new database that copies the schema from the old
First, create the database as we did before. Then go to the name of the database and right click on it and go to Restore as we did before and select the file that we are restoring or paste in the file's full path.  In the upper right corner of the window that pops up you should see an option that says "Restore Options" click on that and check where it says "Only schema".

### Restoring a database that already has the data but you just want to keep the schema 
Go to the name of the database and right click on it and go to Restore as we did before as we did before and select the file that we are restoring or paste in the file's full path. In the upper right corner of the window that pops up you should see an option that says "Restore Options" click on that and check where it says "Only schema". Then check "Clean before restore".
