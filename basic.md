**Q: How can you retrieve all the information from the cd.facilities table?** 

**A:** `SELECT * FROM cd.facilities;`

**Q: You want to print out a list of all of the facilities and their cost to members. How would you retrieve a list of only facility names and costs?**

**A:** `SELECT f.name, f.membercost FROM cd.facilities f;`

**Q: How can you produce a list of facilities that charge a fee to members?**

**A:** `SELECT * FROM cd.facilities WHERE membercost > 0;`

**Q: How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.**

**A:** `SELECT facid, name, membercost, monthlymaintenance FROM cd.facilities WHERE membercost > 0 AND membercost < monthlymaintenance/50;`

**Q: How can you produce a list of all facilities with the word 'Tennis' in their name?**

**A:** `SELECT * FROM cd.facilities WHERE name LIKE '%Tennis%';`

**Q: How can you retrieve the details of facilities with ID 1 and 5? Try to do it without using the OR operator.**

**A:** `SELECT * FROM cd.facilities where facid IN (1,5);`

**Q: How can you produce a list of facilities, with each labelled as 'cheap' or 'expensive' depending on if their monthly maintenance cost is more than $100? Return the name and monthly maintenance of the facilities in question.**

**A:** `SELECT name, CASE WHEN monthlymaintenance > 100 THEN 'expensive' ELSE 'cheap' END AS cost FROM cd.facilities;`

**Notes:** We're doing computation in the area of the query between SELECT and FROM. Previously we've only used this to select columns that we want to return, but you can put anything in here that will produce a single result per returned row - including subqueries.

The CASE statement itself. CASE is effectively like if/switch statements in other languages, with a form as shown in the query. To add a 'middling' option, we would simply insert another when...then section.

**Q: How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, and joindate of the members in question.**

**A:** `SELECT memid, surname, firstname, joindate FROM cd.members WHERE joindate >= '2012-09-01'; `

**Notes:** : The default format for dates in psql is `YYYY-MM-DD HH:MM:SS.nnnnnn`. If you dont use this format and dont specify any format, it will try to infer/convert to this format. Dont leave any room for speculation

**Q: How can you produce an ordered list of the first 10 surnames in the members table? The list must not contain duplicates.**

**A:** `SELECT DISTINCT(surname) FROM cd.members ORDER BY surname ASC LIMIT 10;`

**Q: You, for some reason, want a combined list of all surnames and all facility names. Yes, this is a contrived example :-). Produce that list!**

**A:** `SELECT surname from cd.members UNION SELECT name FROM cd.facilities;`

**Notes:** The UNION operator does what you might expect: combines the results of two SQL queries into a single table. The caveat is that both results from the two queries must have the same number of columns and compatible data types.

UNION removes duplicate rows, while UNION ALL does not. Use UNION ALL by default, unless you care about duplicate results.

**Q: You'd like to get the signup date of your last member. How can you retrieve this information?**

**A:** `SELECT joindate AS latest FROM cd.members ORDER BY joindate DESC LIMIT 1;` --> Less Performant, requires a sort
`SELECT MAX(joindate) AS latest FROM cd.members` --> More Performant

**Q: You'd like to get the first and last name of the last member(s) who signed up - not just the date. How can you do that?**

**A:** `SELECT firstname, surname, joindate FROM cd.members WHERE joindate = (SELECT MAX(joindate) FROM cd.members);`

or

`SELECT firstname, lastname, joindate FROM cd.members ORDER BY joindate ASC LIMIT 1;`

**Notes:** Unclear which is better. Need to look at explain plan


