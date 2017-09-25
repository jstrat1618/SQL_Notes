# Introduction
There should be 3 tables (bookings, facilities, and members) in the cd schema of the excercises database. You query from it as follows:


NOTE: At the time, I did this (mid Sept 2017 the Q&A's were not synced for 12-14. For example, the answers had 15 questions but the answer to question 15 matched my question 15. There also particular dates mentioned in the answers for some of these questions that were not mentioned in the questions.

SELECT * FROM cd.bookings;

# Assessment Test
These questions start off really basic and then get continually more difficult:

1.) How can you retrieve all the information from the cd.facilities table?

SELECT * FROM cd.facilities;

2.) You want to print out a list of all of the facilities and their cost to members. How would you retrieve a list of only facility names and costs?

SELECT name, membercost FROM cd.facilities;

3.)How can you produce a list of facilities that charge a fee to members?

SELECT * FROM cd.facilities
WHERE NOT membercost = 0;

Or

SELECT * FROM cd.facilities
WHERE membercost > 0;


4.) How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.


SELECT name, membercost, monthlymaintenance
FROM cd.facilities
WHERE membercost < monthlymaintenance/50;

5.) How can you produce a list of all facilities with the word 'Tennis' in their name?

SELECT name
FROM cd.facilities
WHERE name LIKE '%Tennis%';

6.) How can you retrieve the details of facilities with ID 1 and 5? Try to do it without using the OR operator.

SELECT * 
FROM cd.facilities
WHERE facid IN (1,5);

7.) How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, and joindate of the members in question.

SELECT memid, firstname, surname, joindate 
FROM cd.members
WHERE joindate >= '2012-09-01';

8.) How can you produce an ordered list of the first 10 surnames in the members table? The list must not contain duplicates.

SELECT DISTINCT(surname)
FROM cd.members
ORDER BY surname
LIMIT 10;

OR

SELECT DISTINCT surname
FROM cd.members
ORDER BY surname
LIMIT 10;

9.) You'd like to get the signup date of your last member. How can you retrieve this information?

SELECT MAX(joindate)
FROM cd.members;

Or if you wanted all the info for the most recent signup.

SELECT * 
FROM cd.members
WHERE joindate = (SELECT MAX(joindate) FROM cd.members);

10.) Produce a count of the number of facilities that have a cost to guests of 10 or more.

SELECT COUNT(guestcost)
FROM cd.facilities
WHERE guestcost >10;

11.) Skip this one, no question for #11.
Produce a list of the total number of slots booked per facility in the month of September 2012. 

12.) Produce an output table consisting of facility id and slots, sorted by the number of slots.
Produce a list of facilities with more than 1000 slots booked. Produce an output table consisting of facility id and total slots, sorted by facility id.

Produce an output table consisting of facility id and slots, sorted by the number of slots.

SELECT facid, SUM(slots) AS num_slots
FROM cd.bookings
GROUP BY facid
ORDER BY num_slots;

Produce a list of facilities with more than 1000 slots booked.

SELECT name, SUM(slots) AS total_slots
FROM cd.bookings
INNER JOIN cd.facilities ON cd.bookings.facid = cd.facilities.facid
GROUP BY name
HAVING SUM(slots) > 1000;

Produce an output table consisting of facility id and total slots, sorted by facility id.

SELECT facid, SUM(slots) AS total_slots
FROM cd.bookings
GROUP BY facid
ORDER BY facid;

REVISED: 12.) Produce a list of the total number of slots booked per facility in the month of September 2012. Produce an output table consisting of facility id and slots, sorted by the number of slots
 
Produce a list of the total number of slots booked per facility in the month of September 2012.

SELECT facid, SUM(slots) AS total_slots
FROM cd.bookings
WHERE starttime >= '2012-09-21' AND starttime < '2012-10-01'
GROUP BY facid
ORDER BY total_slots;

Produce an output table consisting of facility id and slots, sorted by the number of slots

SELECT facid, SUM(slots) AS total_slots
FROM cd.bookings
GROUP BY facid
ORDER BY total_slots;

13.) How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time.


How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'?

SELECT starttime
FROM cd.bookings 
WHERE date_trunc('day', starttime) = '2012-09-21 00:00:00'
ORDER BY starttime;


OR 
SELECT starttime
FROM cd.bookings
WHERE EXTRACT(MONTH FROM starttime) = 9 AND EXTRACT(DAY FROM starttime) = 21 AND EXTRACT(YEAR FROM starttime) = 2012
ORDER BY starttime;


Return a list of start time and facility name pairings, ordered by the time.

SELECT cd.bookings.facid, name, starttime
FROM cd.bookings
INNER JOIN cd.facilities ON cd.facilities.facid = cd.bookings.facid
ORDER BY starttime;

REVISED 13.)Produce a list of facilities with more than 1000 slots booked. Produce an output table consisting of facility id and total slots, sorted by facility id. 

Produce a list of facilities with more than 1000 slots booked.

SELECT facid, SUM(slots) AS total_slots
FROM cd.bookings
GROUP BY facid
HAVING SUM(slots) > 1000
ORDER BY total_slots;

Produce an output table consisting of facility id and total slots, sorted by facility id. 

SELECT facid, SUM(slots) AS total_slots
FROM cd.bookings
GROUP BY facid
HAVING SUM(slots) > 1000
ORDER BY facid;



14.) How can you produce a list of the start times for bookings by members named 'David Farrell'?

Using a subquery, 

SELECT starttime
FROM cd.bookings
WHERE memid = (SELECT memid 
	FROM cd.members
	WHERE surname = 'Farrell' AND firstname = 'David')
ORDER BY starttime;


OR by using a JOIN

SELECT starttime, surname, firstname
FROM cd.bookings
INNER JOIN cd.members ON cd.bookings.memid = cd.members.memid
WHERE firstname = 'David' AND surname = 'Farrell'
ORDER BY starttime;

OR

SELECT starttime, firstname || ' ' || surname AS full_name
FROM cd.bookings
INNER JOIN cd.members ON cd.bookings.memid = cd.members.memid
WHERE firstname = 'David' AND surname = 'Farrell'
ORDER BY starttime;

Or

SELECT starttime, firstname || ' ' || surname AS full_name
FROM cd.bookings
INNER JOIN cd.members ON cd.bookings.memid = cd.members.memid
WHERE firstname || ' ' || surname = 'David Farrell'
ORDER BY starttime;

REVISED 14.) How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time. 
(See Question 13.)

15.) How can you produce a list of the start times for bookings by members named 'David Farrell'? See Question 14.)

