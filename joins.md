**Q: How can you produce a list of the start times for bookings by members named 'David Farrell'?** 

**A:** `SELECT b.starttime FROM cd.bookings b, cd.members m WHERE b.memid = m.memid AND m.firstname = 'David' AND m.surname ='Farrell';`

**Q: How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time.** 

**A:** `SELECT b.starttime, f.name FROM cd.bookings b, cd.facilities f WHERE b.facid = f.facid AND f.name LIKE 'Tennis%' AND DATE(starttime) = '2012-09-21' ORDER BY b.starttime;`

**Q: How can you output a list of all members who have recommended another member? Ensure that there are no duplicates in the list, and that results are ordered by (surname, firstname)**

**A:** `SELECT m.firstname, m.surname FROM cd.members m WHERE m.memid IN (SELECT recommendedby from cd.members) ORDER BY m.surname, m.firstname;`

`SELECT DISTINCT m.firstname, m.surname FROM cd.members m, cd.members m1 WHERE m.memid = m1.recommendedby ORDER BY m.surname, m.firstname;`

**Notes:** Unclear if subquery or join is better. Likely join is better, but do an explain analyze to dig deeper

**Q: How can you output a list of all members, including the individual who recommended them (if any)? Ensure that results are ordered by (surname, firstname).**

**A:** `SELECT m1.firstname AS memfname, m1.surname AS memsname, m2.firstname AS recfname, m2.surname AS recsname FROM cd.members m1 LEFT OUTER JOIN cd.members m2 ON m1.recommendedby = m2.memid ORDER BY m1.surname, m1.firstname;`

**Q: How can you produce a list of all members who have used a tennis court? Include in your output the name of the court, and the name of the member formatted as a single column. Ensure no duplicate data, and order by the member name followed by the facility name.**

**A:** `SELECT DISTINCT (m.firstname || ' ' || m.surname) AS member, f.name AS facility FROM cd.members m INNER JOIN cd.bookings b ON m.memid = b.memid INNER JOIN cd.facilities f ON b.facid = f.facid AND f.name LIKE 'Tennis%' ORDER BY member, facility;`

**Q: How can you produce a list of bookings on the day of 2012-09-14 which will cost the member (or guest) more than $30? Remember that guests have different costs to members (the listed costs are per half-hour 'slot'), and the guest user is always ID 0. Include in your output the name of the facility, the name of the member formatted as a single column, and the cost. Order by descending cost, and do not use any subqueries.**

**A:** `SELECT * FROM (SELECT (m.firstname || ' ' || m.surname) AS member, f.name AS facility, (CASE WHEN m.memid != 0 THEN b.slots*f.membercost ELSE b.slots*f.guestcost END) AS cost FROM cd.members m INNER JOIN cd.bookings b ON m.memid = b.memid INNER JOIN cd.facilities f ON b.facid = f.facid WHERE DATE(b.starttime) = '2012-09-14') tmp where cost > 30 ORDER BY cost DESC;`


**Notes:** https://stackoverflow.com/a/6545685

**Q: How can you output a list of all members, including the individual who recommended them (if any), without using any joins? Ensure that there are no duplicates in the list, and that each firstname + surname pairing is formatted as a column and ordered.**

**A:** `SELECT DISTINCT (m.firstname || ' ' || m.surname) AS member, (SELECT r.firstname || ' ' || r.surname AS recommender from cd.members r where r.memid = m.recommendedby) FROM cd.members m ORDER BY member;`

**Notes:** Subqueries can be in the select clause and join with the main table





