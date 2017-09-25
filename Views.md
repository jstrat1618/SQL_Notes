# Views

A view is a database object that is of a stored query.

A view can be accessible as a virtual table in PostgreSQL

In other words, a PostgreSQL view is a logical table that repressents data of one or more underlying tables through a SELECT statement.

A view helps simplify a complex query because you can query the view (which is based on a complex query)

## Basic syntax
CREATE VIEW view_name AS query;

Here, query represents an actual query.

## Example
SELECT first_name, last_name, address, email, phone
FROM customer 
JOIN address ON customer.address_id = address.address_id;

If we had to keep doing this query on a day to day basis, rather than re-writing it or pasting it we can create a view. Here we'll create a view called customer_info from this query.

CREATE VIEW customer_info AS
SELECT first_name, last_name, address, email, phone
FROM customer 
JOIN address ON customer.address_id = address.address_id;

You should get a message like the one below.

CREATE VIEW

Query returned successfully in 584 msec.

Now, we can query customer_info. For example, we can run the following query.

SELECT * FROM customer_info;

Notice that customer_info is not listed in tables; customer_view is listed under views.

## ALTER VIEW
We can use ALTER VIEW to do this like rename the view. For example, here we will rename the previous view to customer_master;

ALTER VIEW customer_info RENAME TO customer_master;

## DROP VIEW
DROP VIEW view;
You can use the IF XISTS as we did with DROP TABLE.
