
# SQL Syntax
## SELECT Statement
NOTE: Sometimes people say SELECT clause rather than statement, but esentially they are the same thing.

The following illustrates the basic syntax of the SELECT statement.
SELECT column1, column2, ... FROM table_name;

You will notice the SQL language is case insensitive, but by convention SQL keywords are generally in upper case for readability.

NOTE: SQL statements end with a ";"

If you want to query the data from all the columns use the "*" symbol. This is very useful in exploring a table though it is generally not good in application. It can make applications run slower and make the server work harder epecially with bigger data.

### Challenge: Select
We want to send a promotional email to our existing cutsomers for our dvd rental store, and we want to send out a promotion email to our existing customers. Use the SELECT stratement t grab the frist and last names and email address of every customer. 

Solution:
SELECT first_name, last_name, email 
FROM customer;

NOTE: Often times people start a new line with every SQL keyword for readability
SELECT first_name, last_name, email FROM customer; does the same thing since SQL looks for the ";" for the end of a statement.

# SELECT DISTINCT
Learn how to use SELECT with DISTINCT

Syntax:
SELECT DISTINCE columnt
FROM table_name;

e.g. 
SELECT DISTINCT realease_year
FROM film;

You can use multiple columns as well. For example,
SELECT DISTINCT rental_rate, release_year
FROM film;

### Challenge SELECT DISTINCT
Use SELECT DISTINCT to identify the distinct ratings our films can have.

SELECT DISTINCT rating
FROM film;

# SELECT WHERE

Basic Syntax:
SELECT column1, column2, ...
FROM table_name
WHERE condition;

NOTE: the WHERE statement must always be preceeded by a FROM statement.

PosgreSQL Operators (applicable to basically any SQL engine)
=
>
<
>=
<=
<> or !
AND
OR

e.g. Match all customers whose first name is Jamie
SELECT last_name, first_name
FROM customer
WHERE first_name = 'Jamie';

e.g. Match all customers whose first name is Jamie and last name is Rice
SELECT last_name, first_name
FROM customer
WHERE first_name = 'Jamie' and last_name = 'Rice';

e.g. Match all customers who paid more than $8 or less than $1
SELECT customer_id, amount, payment_date
FROM payment
WHERE amount <= 1 OR amount >= 8;

The columns in the WHERE clause don't necessarily have to be in the SELECT clause.

e.g.
SELECT email 
FROM customer
WHERE first_name = 'Jared';		


### Challenge
A customer by the name of Nancy Thomas left her wallet at our store. What's the email address for the custmer named Nancy Thomas?

Solution:
SELECT email 
FROM customer
WHERE first_name = 'Nancy' AND last_name = 'Thomas';

### Challenge
A customer wants to know what the movie "Outlaw Hanky" is about?

Solution: 
SELECT description
FROM film
WHERE title = 'Outlaw Hanky';

### Challenge
A customer is late on the movie rental, and we want to call them. Can you find the phone number for the person who lives at 259 Ipoh Drive

SELECT phone
FROM address
WHERE address = '259 Ipoh Drive';

# The Count Function
The count function returns the number of rows where a certain condition is True

e.g.
SELECT COUNT(*) FROM table;

Here COUNT(*) returns the number of rows returned by the SELECT clause

You can also specify a certain column for readability
e.g. COUNT(col)
However, this does not include NULL values

You can also use the COUNT function with DISTINCT

e.g.
SELECT COUNT(*)
FROM payment;

SELECT COUNT(amount)
FROM payment;


# The Limit statement
The Limit statement does as the name implies and limit the number of rows returned from a query.

e.g.
SELECT *
FROM customer
LIMIT 5;

This query will return at most 5 rows.
SELECT COUNT(DISTINCT amount)
FROM payment;

# ORDER BY Statement
The ORDER BY statement orders a query by a certain column. The syntax is as follows.

SELECT column1, column2
FROM table_name,
ORDER BY column_1 ASC/DESC;

Here ASC (ascending) is the default.


NOTE: An ORDER BY statement is always preceded by a FROM statement.

You can also sort by multiple columns and seperating out each column by a ",".

# examples
SELECT first_name, last_name
FROM customer
ORDER BY first_name;

SELECT first_name, last_name
FROM customer
ORDER BY first_name DESC;

SELECT first_name, last_name
FROM customer
ORDER BY first_name ASC, last_name DESC;

PostgreSQL allows you order by columns even if they are not selected. Other SQL engines will not allow you to do that!

SELECT first_name
FROM customer
ORDER BY last_name;

It's recommend that you always select the column you are ordering by since this will be compatible with other databases.


### Challenge
Get the customer ID number for the top 10 highest paying customers.

Solution:
SELECT customer_id, amount
FROM payment
ORDER BY amount DESC
LIMIT 10; 

### Challenge
Get the title of the movies with film ID's 1-5.

One solution:
SELECT film_id
FROM film
ORDER BY film_id 
LIMIT 5;

Another, I think better, solution:
SELECT film_id
FROM film
WHERE film_id BETWEEN 1 AND 5;

We'll cover BETWEEN, IN and LIKE statements.


# Between Statement
Basic syntax:

variable Between low and high

The between statement is a nice way of writing this out

variable >= low and variable <= high

If you want to see if a variable is out of the range you can use the NOT statement as follows

variable NOT Between low and high

Examples
SELECT * 
FROM payment
WHERE amount BETWEEN 8 AND 9;

SELECT * 
FROM payment
WHERE amount NOT BETWEEN 8 AND 9; 

## Using BETWEEN with dates
You can use between with dates. By default use the format 'YYYY-MM-DD'

Example:
SELECT amount, payment_date 
FROM payment
WHERE payment_date BETWEEN '2007-02-07'  AND '2007-02-15';

# IN Statement
The IN statement can be used to filter a certain variable for 
Basic syntax:

variable IN (value1, value2, ....);

This is the same thing as saying

variable = value1
OR variable = value2
OR .....

PostgresSQL actually executes statements faster using the IN statement rather than a list with a bunch of OR statements (like the one above).

Examples:
SELECT * 
FROM rental
WHERE customer_id IN (1, 2, 3);

SELECT customer_id, rental_id, return_date
FROM rental
WHERE customer_id IN (7, 13, 10);

SELECT *
FROM payment
WHERE amount IN (7.99, 8.99);
ORDER BY return_date DESC;


# LIKE Statement
The LIKE statment helps you filter variables based on a vague description. For example, if we wanted to look up someone in our customer table whose name sounds something like "Jen", perhaps Jennifer, Jenny or something else we can use the LIKE statement as follows

Not all SQL engines support this feature but most due.

This is called pattern matching. You can also use the "_" for matching a single character. The "%" and "_" are called "Wild Card Characters". A summary is given below.

Wild Card Characters
"%"-for matching any sequence of characters
"_"- for matching a single character

Examples: 
SELECT first_name, last_name
FROM customer
WHERE first_name LIKE 'Jen%';

In contrast to the previous example the "%" can actually go at the end. For example, we can find all the names in our database that end with a "y".

SELECT first_name, last_name
FROM customer
WHERE first_name LIKE '%y';

You can even use the "%" to match a sequence at the begining, at the end of a string or somewhere in the middle. For example, we can filter people from our database whose name contains "er" somewhere in the name.

SELECT first_name, last_name
FROM customer
WHERE first_name LIKE '%er%';

As mentioned you can match just a single character using the "_" sign. For example, say we want find the customer names in the customer table whose first name has "her" after the first letter (so it can begin with any single character and end with any string of character but it must end with "h" for the 2nd character, "e" for the 3rd and "r" for the 5th.

SELECT first_name, last_name
FROM customer
WHERE first_name LIKE '_her%';

## Case sensitivity
The LIKE statement is case sensitive, whereas ILIKE is case sensisitive. For example, in search for names that begin with "BAR" LIKE returns no rows whereas ILIKE returns 2 rows. Run the queries below:

SELECT first_name, last_name
FROM customer
WHERE first_name LIKE 'BAR%';  

SELECT first_name, last_name
FROM customer
WHERE first_name ILIKE 'BAR%';


# Challenge
How many payment transactions were greater than $5.00?

Solution:
SELECT COUNT(*) amount 
FROM payment
WHERE amount > 5;

How many actors have a first name that starts with P?

Solution
SELECT first_name 
FROM actor
WHERE first_name LIKE 'P%';

How many unique districts are our customers from?

Solution:
SELECT COUNT(DISTINCT(district))
FROM address;

Retrieve the list of names of the district from those above.


Solution:
SELECT DISTINCT(district)
FROM address;

OR

SELECT DISTINCT district
FROM address;

How may films have a rating of R and cost between $5 and $15?

Solution:
SELECT COUNT(*)
FROM film 
WHERE rating = 'R' AND replacement_cost BETWEEN 5 AND 15;


How many films have the word "Truman" somewhere in the title?

Solution:

SELECT COUNT(*)
FROM film
WHERE title LIKE '%Truman%';

OR

SELECT COUNT(title)
FROM film
WHERE title LIKE '%Truman%';

# Aggregate Functions- MIN, MAX, AVG, SUM

### Note: COUNT is also an aggregate function but we already discussed it

Examples:
## AVG
SELECT AVG(amount) FROM payment;

### To round we can use the ROUND statement
SELECT ROUND(AVG(amount),3) FROM payment;

## SUM
SELECT SUM(amount)
FROM payment;

## MIN
SELECT MIN(amount)
FROM payment;

## MAX
SELECT MAX(amount)

# GROUP BY
## Basic syntax
SELECT column1, AVG(column2) 
FROM table_name
GROUP BY column1;

## Example
SELECT column1, AVG(column2) 
FROM table_name
GROUP BY column1;

SELECT customer_id, SUM(customer_id) 
FROM payment
GROUP BY customer_id;

### ten most valuable customers
SELECT customer_id, SUM(amount) 
FROM payment
GROUP BY customer_id
ORDER BY SUM(amount) DESC
LIMIT 10;

### Note: PostgreSQL is very flexible in that it doesn't require you to select the column that you are grouping by. However, most other SQL engines require you to select that column. Similar to ORDER BY

### Note: Other SQL engines may also require that you have an arregate function with GROUP BY, but not PostgreSQL

## Other Examples using COUNT
Who has been processing the most orders?

SELECT staff_id, COUNT(*)
FROM payment
GROUP BY staff_id;

Using the film table, how many films of each rating type do we have?

SELECT rating, COUNT(*)
FROM film
GROUP BY rating;

What about rental duruations?

SELECT rental_duration, COUNT(*)
FROM film
GROUP BY rental_duration;

Let's say we want to know the average rental_rate for each rating.

SELECT rating, AVG(rental_rate)
FROM film
GROUP BY rating;


# GROUP BY Challenge
## 1st Challenge
We have two staff members with staff IDs 1 and 2. We want to give a bonus to the staff member that handled the most payments. How many payments did each staff member handle? And how much was the total amount processed by each staff member?

## Solution
SELECT staff_id, COUNT(*), SUM(amount)
FROM payment
GROUP BY staff_id;

## 2nd Challenge
Corporate HQ is auditing our store. They want to know the average replacement costs of movies by rating.

## Solution
SELECT rating, AVG(replacement_cost)
FROM film
GROUP BY rating;

## 3rd Challenge
We want to send coupons to 5 custermers who have spend the most amount of money. Get me the customer ids of the top 5 customer spenders.

## Solution
SELECT customer_id, SUM(amount)
FROM payment
GROUP BY customer_id
ORDER BY SUM(amount) DESC
LIMIT 5;

# HAVING Statement
We often use the HAING clause in conjunction with the GROUP BY clause to filter group rows that do not satisfy a specified condition.

Note: You cannot use WHERE with a GROUP BY clause, unless it appears in a subquery (See AdvancedSQLCommands).

## Basic syntax
SELECT col1, aggrefate_function(col2)
FROM table_name
GROUP BY col1
HAVING condition;

### Example
Suppose we want to see the cusomer id's for the customers who have spend at least $200.

SELECT customer_id, SUM(amount)
FROM payment
GROUP BY customer_id
HAVING SUM(amount) > 200;

Suppose we want to know which stores have more than 300 customers customers.

SELECT store_id, COUNT(customer_id)
FROM customer
GROUP BY store_id
HAVING COUNT(customer_id) >= 300;

### Example using WHERE and HAVING
Find the average rental_rate for R, G, and PG movies that have an average rental rate less than 3.

SELECT rating, AVG(rental_rate) 
FROM film
WHERE rating IN ('R', 'G', 'PG')
GROUP BY rating
HAVING AVG(rental_rate) < 3;

# HAVING Challenge!

## 1st Challenge
We want to know what customers are eligible for our platinum credit card that requires customer to have at least a total of 40 transaction payments?

## Solution
SELECT customer_id, COUNT(customer_id) 
FROM payment
GROUP BY customer_id
HAVING COUNT(customer_id) >= 40;

## 2nd Challenge
When group by rating what movie rating have an average rental duration of more than 5 days?


## Solution
SELECT rating, AVG(rental_duration)
FROM film
GROUP BY rating
HAVING AVG(rental_duration) > 5;