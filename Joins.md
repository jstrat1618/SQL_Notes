# The AS statement
AS allows us to rename columns or table selection with an alias

## Basic Syntax
SELECT var1 AS new_name4_var1
FROM table_name;

## Example
SELECT payment_id AS payment_id_num FROM payment;

## Another example
SELECT customer_id, SUM(amount) AS total_spent
FROM payment
GROUP BY customer_id;

# Joins
* Joins allow you to relate data that may be in different tables
* There are several kinds of joins including INNER JOIN, OUTER JOIN and more

# INNER JOIN
Suppose you want to get data from two tables named A and B.

Table B as the foreign key (fka) that relates the the primary key (pka) in table A.

An INNER JOIN clause returns rows in the A table that have the corresponding rows in the B table. (Think of the intersection between the two data sets).

## Basic syntax of INNER JOIN
SELECT A.pka, A.col1, B.pkb, B.col2
FROM A 
INNER JOIN B ON A.pka = B.fka

### Breakdown of syntax
* First, you specify the columns from both tables which you want to select data in the SELECT clause
* Second, you specify the main table (in this case A) in the FROM clause
* Third, you specify the table that the main table joins to (in this case B) in the INNER JOIN clause. Additionally, you specify the condition after the ON keyword (in this case A.pka = B.fka).


## Example using the customer and payment tables

SELECT customer.customer_id, first_name, last_name, email, amount, payment_date
FROM customer
INNER JOIN payment ON payment.customer_id = customer.customer_id;

#### Order by customer_id
SELECT customer.customer_id, first_name, last_name, email, amount, payment_date
FROM customer
INNER JOIN payment ON payment.customer_id = customer.customer_id
ORDER BY customer.customer_id;

#### Order by first_name
SELECT customer.customer_id, first_name, last_name, email, amount, payment_date
FROM customer
INNER JOIN payment ON payment.customer_id = customer.customer_id
ORDER BY customer.customer_id;

#### Filter to show only data for customer_id 2
SELECT customer.customer_id, first_name, last_name, email, amount, payment_date
FROM customer
INNER JOIN payment ON payment.customer_id = customer.customer_id
WHERE customer.customer_id =2;


#### Filter to show only customers whose first names start with an "A"
SELECT customer.customer_id, first_name, last_name, email, amount, payment_date
FROM customer
INNER JOIN payment ON payment.customer_id = customer.customer_id
WHERE first_name LIKE 'A%';

## More examples

SELECT payment_id, amount, first_name, last_name
FROM payment
INNER JOIN staff ON payment.staff_id = staff.staff_id;

### NOTE: Most (not all SQL engines treat JOIN as INNER JOIN- this includes PostgreSQL)

SELECT store_id, title 
FROM inventory
JOIN film ON inventory.film_id = film.film_id;

### What if we want to know how many copies of each movie are in store_id 1?

SELECT title, COUNT(title)
FROM inventory
JOIN film ON inventory.film_id = film.film_id
WHERE store_id =1
GROUP BY title;

### Using the film and language tables what language each film is in.

SELECT film.title, language.name AS movie_language
FROM film
INNER JOIN language ON language.language_id = film.language_id;

### Note: It is not necessary to use the AS statement with a lot of SQL engines (including PostgreSQL) instead you can just put a space as follows.

SELECT film.title, language.name  movie_language
FROM film
INNER JOIN language ON language.language_id = film.language_id;

# LEFT OUTER JOIN
Note: For the left outer join and outer joins in general you can just write LEFT JOIN instead of LEFT OUTER JOIN

## Basic syntax
SELECT A.pka, A.col1, B.pkb, B.col2
FROM A 
LEFT OUT JOIN B ON A.pka = B.fka

## Example
SELECT film.film_id, title, inventory_id
FROM film
LEFT JOIN inventory ON film.film_id = inventory.film_id;

After running the above example, if you scroll down to about 4600 rows, you'll notice that some of the rows don't have a value for intevntory_id. This is because some of the films are not in inventory They appear because we did a LEFT JOIN, had we done an INNER JOIN they wouldn't be there. 

Imagine we only wanted films that are not in our inventory. We could do this by adding a WHERE clause.

SELECT film.film_id, title, inventory_id
FROM film
LEFT JOIN inventory ON film.film_id = inventory.film_id
WHERE inventory.film_id IS NULL;

SELECT film.film_id, title, inventory_id
FROM film
LEFT JOIN inventory ON film.film_id = inventory.film_id
WHERE inventory.film_id IS NULL;

Alternatively, you can use inventory_id

SELECT film.film_id, title, inventory_id
FROM film
LEFT JOIN inventory ON film.film_id = inventory.film_id
WHERE inventory_id IS NULL;

# UNION
The union operator combines the result sets of two or more SELECT statements intoa  single result set.

Basic Syntax:
SELECT col1, col2 
FROM tbl_name1
UNION
SELECT col1, col2
FROM tbl_name2;

There are two rules to follow when doing queries that use a UNION statement:
1.) Both queries must return the same number of columns.
2.) The corresponding columns in the queries must have compatible data types.

We most often use UNION in the following 2 scenarios:
1.) We want to combine data from similar tables that are not perfectly normalized
2.) Those tables are often found in the reporting or data warehouse systems.

Example: Suppose we had to tables as follows
salesQ1
Name, amount
Jon, 3500
Mike, 3600
Mary, 4000

saleSQ2
Name, amount
Jon, 4600
Mike, 4400
Mary, 4000

We could combine the table with the following UNION statement
SELECT * FROM salesQ1
UNION 
SELECT * FROM salesQ2

The resulting table would look like this:
Name, amount
Jon, 3500
Mike, 3600
Mary, 4000
Name, amount
Jon, 4600
Mike, 4400

NOTICE MARY only appears once this is because she sold the same amount in both quarters and UNION deletes duplicated rows. However, if you wanted to keep all the rows you could use a UNION ALL statement as follows

SELECT * FROM salesQ1
UNION 
SELECT * FROM salesQ2

This would produce the following table

Name, amount
Jon, 3500
Mike, 3600
Mary, 4000
Jon, 4600
Mike, 4400
Mary, 4000

