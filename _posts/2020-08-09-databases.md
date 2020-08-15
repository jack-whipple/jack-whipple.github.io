---
title: "Databases Artifact"
date: 2020-08-09
tags: [databases, computer science, mysql]
header:
  image: "/images/mysql_table.png"
excerpt: "Databases, Computer Science, MySQL"
---

<h3><p align="center">Databases Artifact Narrative</p></h3>

The purpose of this narrative is to reflect on the artifact I selected for this category and the process I experienced in creating and it. Throughout this narrative I will describe the artifact I selected, explain why its inclusion in my ePortfolio is justified, how I am meeting course outcomes with this artifact, and finally reflect on the process of creating this artifact and share the lessons and challenges I had while working on this project.

##### Describe the Artifact
The artifact I chose for this category is a MySQL program (now called CS 499 Databases.sql) that comes from the SNHU class DAD 220: Introduction to SQL. I originally created this artifact in 2018, and it was designed for a scenario in which I was tasked with performing CRUD operations for a messaging database that included 4 different tables. The original file already accounts for the tables person and contact_list already existing prior to execution of this .sql file. This MySQL program focuses on Altering a table, updating a table, deleting data from a table, creating two new tables, and performing three different queries.

##### Justify Artifact Inclusion
I chose this artifact for my ePortfolio because it aligns with basic Database principles and demonstrates MySQL advanced concepts. This program uses standard MySQL commands to store and manipulate data, and demonstrates my ability to work with and develop a database that includes multiple tables with various column fields that are used to store and manipulate data. This artifact also shows my ability to perform queries on the database in order to perform a search of information based on specific requirements. That was specifically noticeable in the query to find messages with at least one image attached. The enhancements I made to this artifact show the ability to employ advanced MySQL concepts to the database in order to make it more manageable and to give it more functionality for future use. The enhanced version of this program has changed the original queries into MySQL Views so that the queries can easily be repeated in the future by only running a query in the form of SELECT * FROM [database view name], and the image table was altered to have full-text indexing so that full-text searches could be performed.

##### Course Objectives
The enhancements I made to this artifact have met course objectives that I had planned on in the ePortfolio selection journal. The enhancements I implemented demonstrates my ability to use innovative skills and techniques for implementing database solutions. This was specifically addressed by the MySQL command line statements that perform CRUD operations in the messaging database, and the advanced MySQL concepts show the ability to implement advanced database solutions to achieve project goals. The enhanced artifact also demonstrates my ability to program solutions to solve problems involving storing, manipulating, or accessing data. This was demonstrated by my use of creating the image and message_image tables, Altering the person and image tables, updating the person table, and deleting a person from the person table. Finally, my use of MySQL Views and MySQL Full-Text shows a more advanced way of accessing the data stored within the database.

##### Artifact Reflection
I love working with databases, and I am always excited to learn new ideas and concepts in relation to database development and management. This artifact was the easiest of the three for me to enhance, and I did not encounter any challenges or difficulties in enhancing the program. While simple, the advanced concepts I used to enhance this program are not about the complexity in the written command statements, but more about the advanced functionality and enhancement it brings to users. I learned that these two enhancements make it easier for users to perform database operations, and sometimes enhancing a project is not all about what you write in the script, but the program’s behavior and functionality for a user. This program was enhanced to make repeated queries easier for users, and to give users the ability to search for data based on key words rather than exact values. This type of search would be very valuable when searching through documents that contain a lot of text-based values in their columns. While this artifact was not very complicated to enhance it was favorite artifact to work on for this capstone course.


### Databases Artifact:
```sql
/* Orginally created by Jack Whipple for DAD 220 (Introduction to SQL). Created in Dec 2018.
 * Updated August 2nd, 2020 by Jack Whipple for CS 499 (Computer Science Capstone).
 */

USE messaging;

/* Instert record into the person table. 
 * I will insert myself into this table.
 */
INSERT INTO person (first_name, last_name)
VALUES ('Jack', 'Whipple');

/* Alter the person table to add a custom table. 
 * I will add a gender column for each person.
 */
ALTER TABLE person
ADD gender VARCHAR(10) NOT NULL;

/* Update my record in the person table. 
 * Input a new value into the gender column.
 */
UPDATE person
SET gender = 'male'
WHERE person_id = 7;

/* Delete records from person table. 
 * Will DELETE the record for Diana Taurasi.
 */
DELETE FROM person
WHERE first_name = 'Diana' AND
last_name = 'Taurasi';

/* Alter the contact_list table. 
 * Include a new column named 'favorite' with data 
 * type VARCHAR(10) and not required.
 */
ALTER TABLE contact_list
ADD favorite VARCHAR(10) DEFAULT NULL;

/* Update the record in contact_list and set 
 * everyone's favorite contact to 'Michael Phelps'.
 */
UPDATE contact_list
SET favorite = 'y'
WHERE contact_id = 1;

/* Update records for the contact_list. 
 * Set everyone who is not 'Michael Phelps' to not be a favorite.
 */
UPDATE contact_list
SET favorite = 'n'
WHERE contact_id <> 1;

/* Insert new records into the contact_list. 
 * Create three new contacts for this table which 
 * will include myself.
 */
INSERT INTO contact_list (person_id, contact_id, favorite)
VALUES (7, 1, 'y'),
(7, 3, 'n'),
(7, 5, 'y'),
(1, 7, 'y'),
(2, 7, 'n'),
(4, 7, 'y');

/* Create the new image table.*/
CREATE TABLE image (
image_id INT(8) NOT NULL auto_increment,
image_name VARCHAR(50) NOT NULL,
image_location VARCHAR(250) NOT NULL,
PRIMARY KEY (image_id)    
)AUTO_INCREMENT = 1;

/* Create the new message_image table.*/
CREATE TABLE message_image (
message_id INT(8) NOT NULL,
image_id INT(8) NOT NULL,
PRIMARY KEY (message_id, image_id)
);

/* Insert new records into the image table. 
 * Create at least five new records.
 */
INSERT INTO image (image_name, image_location)
VALUES ('Final Swim Race', 'Rio De Janeiro'),
('Winning Gold', 'Rio De Janeiro'),
('Fastest Man Alive', 'London, England'),
('Nothing but Gold', 'Rio De Janeiro'),
('Medal Ceremony', 'Rio De Janeiro'),
('Closing Ceremony', 'Rio De Janeiro');

/* Insert records into the message_image table. 
 * Create at least five new records.
 */
INSERT INTO message_image (message_id, image_id)
VALUES(1, 2),
(2, 1),
(2, 4),
(3, 5),
(4, 3);

--ALL CS 499 Artifact 3 Enhacments are below this line (AUGUST 2, 2020)

/* Find all of the messages that 'Michael Phelps' sent. 
 * Create a SQL query.
 *
 * DAD 220 Orginal Project Query:
SELECT
	s.first_name AS "Sender's first name",
	s.last_name AS "Sender's last name",
	r.first_name AS "Receiver's frist name",
	r.last_name AS "Reciever's last name",
	m.message_id AS "Message ID",
	m.message AS "Message",
	m.send_datetime AS "Message timestamp"
FROM
	person s,
	person r,
	message m
WHERE 
	m.sender_id = s.person_id AND
	m.receiver_id = r.person_id AND
	s.first_name = 'Michael' AND
	s.last_name = 'Phelps';
 */

/* Finad all of the Messages that 'Michale Phelps' sent.
 * This will create a database view that can be ran repeatdly without 
 * having to write out the query every time.
 * 
 * CS 499 MySQL View Enhancement:
 */
CREATE VIEW michaelPhelpsMessages AS
	SELECT
		s.first_name AS "Sender's first name",
		s.last_name AS "Sender's last name",
		r.first_name AS "Receiver's frist name",
		r.last_name AS "Reciever's last name",
		m.message_id AS "Message ID",
		m.message AS "Message",
		m.send_datetime AS "Message timestamp"
	FROM
		person s,
		person r,
		message m
	WHERE 
		m.sender_id = s.person_id AND
		m.receiver_id = r.person_id AND
		s.first_name = 'Michael' AND
		s.last_name = 'Phelps';

/* Find the number of messages sent for every person.
 * Create a SQL query.
 *
 * DAD 220 Orginal Project Query:

SELECT
	COUNT(m.sender_id) AS "Message count",
	p.person_id AS "Person ID",
	p.first_name AS "First name",
	p.last_name AS "Last name"
FROM
	message m,
	person p
WHERE 
	m.sender_id = p.person_id
GROUP BY
	p.person_id,
	p.first_name,
	p.last_name;
 */	

/* Find the number of messages sent for every person.
 * This will create a database view that can be ran repeatdly without 
 * having to write out the query every time.
 * 
 * CS 499 MySQL View Enhancement:
 */	
CREATE VIEW numberOfMessagesSent AS
	SELECT
		COUNT(m.sender_id) AS "Message count",
		p.person_id AS "Person ID",
		p.first_name AS "First name",
		p.last_name AS "Last name"
	FROM
		message m,
		person p
	WHERE 
		m.sender_id = p.person_id
	GROUP BY
		p.person_id,
		p.first_name,
		p.last_name;
 	

/* Find all the messages that have at least one image
 * attached. Create SQL query using INNER JOINs.
 *
 * DAD 220 Orginal Project Query:
SELECT
	m_i.message_id AS "Message ID",
	MIN(m.message) AS "Message",
	MIN(m.send_datetime) AS "Message timestamp",
	MIN(i.image_name) AS "Image name",
	MIN(i.image_location) as "Image location"
FROM
	message m
INNER JOIN message_image m_i ON m.message_id = m_i.message_id
INNER JOIN image i ON i.image_id = m_i.image_id 
GROUP BY m_i.message_id;
 */
 
 /* Find all the messagesthat have at least one image attached.
 * This will create a database view that can be ran repeatdly without 
 * having to write out the query every time.
 * 
 * CS 499 MySQL View Enhancement:
 */
 CREATE VIEW messagesWithImage AS	
	SELECT
		m_i.message_id AS "Message ID",
		MIN(m.message) AS "Message",
		MIN(m.send_datetime) AS "Message timestamp",
		MIN(i.image_name) AS "Image name",
		MIN(i.image_location) as "Image location"
	FROM
		message m
	INNER JOIN message_image m_i ON m.message_id = m_i.message_id
	INNER JOIN image i ON i.image_id = m_i.image_id 
	GROUP BY m_i.message_id;
	
/* Create FULLTEXT index using ALTER TABLE statement
 * This is to index the data in the image table and prepare it for
 * full-text searches.
 *
 * CS 499 MySQL Full-Text Index enhancement
 */
ALTER TABLE image
ADD FULLTEXT(image_name, image_location);

/* Using Full-Text Search to find images that contins the word "Rio"
 * in the image_location field. Order by image_name.
 * 
 * CS 499 MySQL Full-Text Search Enhancement
 */
 SELECT
 	image_name,
	image_location
 FROM
 	image
 WHERE
 	MATCH(image_location)
	AGAINST('Rio')
 ORDER BY image_name;				

```
