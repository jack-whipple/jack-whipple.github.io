---
title: "Databases Artifact"
date: 2020-08-09
tags: [databases, computer science, mysql]
header:
  image: "/images/mysql.png"
excerpt: "Databases, Computer Science, MySQL"
---

Greetings!

This project is my Databases Artifact for CS 499. This program was orginally created to demonstrate my ability to use basic MySQL commands to perform CRUD operations on a database for a fictitious secnario.
The orginal .sql file could be excuted in any terminal running MySQL and after execution would perform CRUD operations, and then perform three different querries. For my CS 499 enhancement I decided to improve the .sql file
by adding advanced MySQL concepts. The advanced concepts I added are MySQL views, and MySQL Full-text search. I made the three original queries into views so that a user could more easily run the queries over and over in the future,
and I added full-text search to the images table to allow a user to more easily search for an image name or location based on a key word.

### Databases Artifact:
```mysql
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
