**Q: The club is adding a new facility - a spa. We need to add it into the facilities table. Use the following values:**

* **facid: 9, Name: 'Spa', membercost: 20, guestcost: 30, initialoutlay: 100000, monthlymaintenance: 800** 

**A:** `INSERT INTO cd.facilities VALUES (9, 'Spa', 20, 30, 100000, 800);`

**Q: In the previous exercise, you learned how to add a facility. Now you're going to add multiple facilities in one command. Use the following values:**

* **facid: 9, Name: 'Spa', membercost: 20, guestcost: 30, initialoutlay: 100000, monthlymaintenance: 800** 
* **facid: 10, Name: 'Squash Court 2', membercost: 3.5, guestcost: 17.5, initialoutlay: 5000, monthlymaintenance: 80**

**A:** `INSERT INTO cd.facilities VALUES (9, 'Spa', 20, 30, 100000, 800), (10, 'Squash Court 2', 3.5, 17.5, 5000, 80);`


**Q: Let's try adding the spa to the facilities table again. This time, though, we want to automatically generate the value for the next facid, rather than specifying it as a constant. Use the following values for everything else:**

* **Name: 'Spa', membercost: 20, guestcost: 30, initialoutlay: 100000, monthlymaintenance: 800** 

**A:** `INSERT INTO cd.facilities SELECT (SELECT MAX(facid) + 1 FROM cd.facilities), 'Spa', 20, 30, 100000, 800;`


**Q: We made a mistake when entering the data for the second tennis court. The initial outlay was 10000 rather than 8000: you need to alter the data to fix the error**

**A:** `UPDATE cd.facilities SET initialoutlay = 10000 WHERE initialoutlay = 8000 AND name = 'Tennis Court 2';`


**Q: We want to increase the price of the tennis courts for both members and guests. Update the costs to be 6 for members, and 30 for guests.**

**A:** `UPDATE cd.facilities SET membercost=6, guestcost=30 WHERE name LIKE 'Tennis%';`

**Q: We want to alter the price of the second tennis court so that it costs 10% more than the first one. Try to do this without using constant values for the prices, so that we can reuse the statement if we want to.**

**A:** `UPDATE cd.facilities SET membercost = (SELECT membercost FROM cd.facilities WHERE facid = 0)*1.1, guestcost = (SELECT guestcost FROM cd.facilities WHERE facid = 0)*1.1 WHERE facid = 1;;`

`UPDATE cd.facilities f SET membercost = f1.membercost*1.1, guestcost = f1.guestcost*1.1 FROM (SELECT membercost, guestcost FROM cd.facilities WHERE facid=0) f1 WHERE f.facid = 1;`


**Notes:** Can use Update FROM syntax

**Q: As part of a clearout of our database, we want to delete all bookings from the cd.bookings table. How can we accomplish this?**

**A:** `DELETE FROM cd.bookings;`

**Notes:** TRUNCATE also deletes everything in the table in a non qualified way, but does so using a quicker underlying mechanism. It's not perfectly safe in all circumstances, though, so use judiciously. When in doubt, use DELETE.

**Q: We want to remove member 37, who has never made a booking, from our database. How can we achieve that?**

**A:** `DELETE FROM cd.members WHERE memid = 37;`

**Q: In our previous exercises, we deleted a specific member who had never made a booking. How can we make that more general, to delete all members who have never made a booking?**

**A:** `DELETE FROM cd.members WHERE memid NOT IN (SELECT memid FROM cd.bookings);`

`DELETE FROM cd.members m WHERE NOT EXISTS (SELECT 1 FROM cd.bookings b WHERE b.memid = m.memid);`


