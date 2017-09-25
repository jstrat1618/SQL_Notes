# Timestamps
Timestamps allow us to retain time information. Right now, we'll focus on extracing info from a timestamp and later learn how to create a timestamp. See the https://www.postgresql.org/docs/9.6/static/functions-datetime.html page for a thorough overview of timestamp functions.

In this documenatation you'll notice lots of different functions like age, clock_tiestamp, date_part and so forth that do lots of different things.

Note: Timestamps and much of the content here syntactically differ among SQL engines.

Note: You can use the artithmatic operators for datetime objects in postgreSQl.

## extract function
You can extract different elements from a timestamp object using the extract function.

### Basic syntax
SELECT (extract component_of_timestamp from timestamp_object) FROM tbl_name;

### Example
SELECT extract(day from payment_date) 
FROM payment;

Let's rename the extracted column in the above example.

SELECT extract(day from payment_date) AS day
FROM payment;

### Another example
We can look at the totals paid by month using the payment table as follows.

SELECT extract(month from payment_date) AS month, SUM(amount) AS total
FROM payment
GROUP BY month;

What was the highest grossing month?

SELECT extract(month from payment_date) AS month, SUM(amount) AS total
FROM payment
GROUP BY month
ORDER BY total DESC
LIMIT 1;

-Result is month 4 (April)

# Mathematical Functions
There are a variety of mathematical functions available in posgreSQL. Here are some of the mathematical functions available- https://www.postgresql.org/docs/current/static/functions-math.html.

## Example
SELECT rental_id + customer_id AS x 
FROM payment;

SELECT rental_id / customer_id AS x 
FROM payment;

# String functions
There are lots of string functions available in postgreSQL documented here: https://www.postgresql.org/docs/current/static/functions-string.html.

These functions do things like concatenates strings, changes the case of a string, compute the length of string and so forth.

## Example of Concatentation
SELECT first_name || last_name AS full_name 
FROM customer;

We can add a space in the previous query to obtain a more clearly written name.

SELECT first_name || ' ' || last_name AS full_name 
FROM customer;

## Example of changing the case
SELECT UPPER(first_name), lower(last_name)
FROM customer;


Note: you can also use regular expressions in posgreSQL.

	
# Subqueries
A subquery uses multiple SELECT statements to do a query within a query. Subqueries go inside paranetheses, and they are often used inside something like a WHERE clause.

SQL engines first exectue the subquery then then passes the result to the outer query and then executes the outer query.


## Example
Suppose we want to get films whose rental rate is higher than the average rental rate.

You could do this in two seperate queries as follows:

SELECT UPPER(first_name), lower(last_name)
FROM customer;

The result is 2.98, and using this you can run the following query. Now, you can run the following query.

SELECT title, rental_rate
FROM film
WHERE rental_rate > 2.98;

Using a subquery we can do this as follows:

SELECT film_id, title, rental_rate
FROM film
WHERE rental_rate > (SELECT AVG(rental_rate) FROM film);

### Another example
Suppose we want the film_id's  that were returned between May 29th, 2005 and May 30th, 2009.

SELECT inventory.film_id 
FROM rental
INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
WHERE 
return_date BETWEEN '2005-05-29' AND '2005-05-30';

We can now use this query as a subquery to get the film_id's and the movie title

SELECT title, film_id
FROM film
WHERE film_id IN
	(SELECT inventory.film_id 
	FROM rental
	INNER JOIN inventory ON inventory.inventory_id = rental.inventory_id
	WHERE 
	return_date BETWEEN '2005-05-29' AND '2005-05-30');

# Self Joins
You use self join when you want to combine rows with other rows in the same table.

To do a self join you must use an alias to distinguish the left table from the right.

Suppose we have the following table called employee.
employee_name, employee_location
Joe, New York
Sunil, India
Alex, Russia
Albert, Canada
Jack, New York

Suppose you wanted to find all the employees in the employee table that have the same location as Joe, but we cannot hard code "New York" into our query (perhaps because the location may change or for whatever reason).

One approach is to do a subquery as follows

SELECT employee_name, location FROM employee
FROM employee
WHERE location IN 
	(SELECT location
	FROM employee
	WHERE employee_name = 'Joe');

However, it is actually more efficient to use a self join.

SELECT e1.employee_name 
FROM employee AS e1, employee as e2
WHERE e1.employee_location = e2.employee_location
AND e2.employee_name = "Joe";

This would result in the following table
e1.employee_name, e1.employee location, e2.employee_name, e2.employee_location
Joe, New York, Joe, New York
Jack, New York, Joe, New Yorks

## Example
Using the customer tabe let's find out which customers have whose last name matches a first name of another customer. (e.g. a name like Sandra Martin would be returned from this query iff a name like Martin Smith is in the table).

SELECT a.first_name, a.last_name, b.first_name, b.last_name
FROM customer AS a, customer AS b
WHERE a.last_name = b.first_name;

You can also do a self join as an INNER JOIN by using the following. For example, we could do the previous example as follows:

SELECT a.first_name, a.last_name, b.first_name, b.last_name
FROM customer AS a
INNER JOIN customer AS b ON a.last_name = b.first_name;

You can also do other join. While the examples we've seen so far really make the most sense with an INNER JOIN you can easily change that. For example, suppose we wanted to do a LEFT JOIN from the example above, we just change INNER to LEFT.

SELECT a.first_name, a.last_name, b.first_name, b.last_name
FROM customer AS a
LEFT JOIN customer AS b ON a.last_name = b.first_name;


A really common interview question on SQL is 
the so called "manager to employee self join". Check it out!

