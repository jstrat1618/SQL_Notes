# Question 1
Return the customer IDs of customers who have spent at least $110 with the staff member who has an ID of 2.

# Solution
SELECT customer_id, staff_id, SUM(amount)
FROM payment
GROUP BY customer_id, staff_id
HAVING staff_id = 2 AND SUM(amount) >= 110;

# Question 2
2. How many films begin with the letter J?

# Solution:
SELECT first_name, last_name, customer_id, address_id
FROM customer
WHERE first_name LIKE 'E%' AND address_id < 500
ORDER BY customer_id DESC
LIMIT 1;