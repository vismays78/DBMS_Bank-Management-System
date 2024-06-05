# DBMS_Bank-Management-System

This repository contains a MySQL database setup and operations for a simulated bank (HDFC Bank) including the creation of various tables, insertion of sample data, execution of queries, creation of views, and implementation of triggers.

# Database Structure

The database consists of the following tables:
- branch: Contains information about different bank branches.
- customer: Contains information about bank customers.
- account: Contains information about bank accounts.
- loan: Contains information about loans offered by the bank.
- availed: A mapping table that links customers to the loans they have availed.
- hold: A mapping table that links customers to the accounts they hold.

# Table Definitions
# branch
sql
CREATE TABLE branch (
    bid INT PRIMARY KEY,
    bname VARCHAR(255),
    badd VARCHAR(255)
);


# customer
sql
CREATE TABLE customer (
    cid INT PRIMARY KEY,
    cname VARCHAR(30),
    cphone INT,
    cadd VARCHAR(255)
);


# account
sql
CREATE TABLE account (
    accno INT PRIMARY KEY,
    acctype VARCHAR(50),
    accbalance INT,
    bid INT REFERENCES branch
);


# loan
sql
CREATE TABLE loan (
    lid INT PRIMARY KEY,
    ltype VARCHAR(50),
    lamt INT,
    bid INT REFERENCES branch
);


# availed
sql
CREATE TABLE availed (
    lid INT REFERENCES loan,
    cid INT REFERENCES customer
);


# hold
sql
CREATE TABLE hold (
    accno INT REFERENCES account,
    cid INT REFERENCES customer
);


# Sample Data
The database is populated with the following sample data:

# branch
sql
INSERT INTO branch VALUES (123, 'andheri', 'mumbai'), 
                          (234, 'ghatkopar', 'mumbai'), 
                          (345, 'sarojini nagar', 'delhi'), 
                          (456, 'bhayander', 'thane'), 
                          (567, 'badlapur', 'thane'), 
                          (678, 'versova', 'mumbai'), 
                          (789, 'byculla', 'mumbai'), 
                          (891, 'dadar', 'mumbai');


# customer
sql
INSERT INTO customer VALUES (1, 'vismay', 756329, 'versova'), 
                            (2, 'raghav', 362836, 'andheri'), 
                            (3, 'darshil', 927537, 'bhiwandi'), 
                            (4, 'ritesh', 192736, 'byculla'), 
                            (5, 'shanay', 393726, 'surat');


# account
sql
INSERT INTO account VALUES (111, 'savings', 10000, 123), 
                           (222, 'savings', 20000, 456), 
                           (333, 'current', 40000, 345), 
                           (444, 'current', 5000, 123), 
                           (555, 'current', 50000, 234), 
                           (666, 'current', 70000, 567), 
                           (777, 'savings', 90000, 891), 
                           (888, 'current', 100000, 789);


# loan
sql
INSERT INTO loan VALUES (01, 'home loan', 100000, 123), 
                        (02, 'business loan', 200000, 234), 
                        (03, 'car loan', 300000, 345), 
                        (04, 'gold loan', 500000, 678), 
                        (05, 'education loan', 400000, 891);


# availed
sql
INSERT INTO availed VALUES (01, 1), 
                           (03, 3), 
                           (04, 4), 
                           (05, 5);


# hold
sql
INSERT INTO hold VALUES (111, 1), 
                        (222, 2), 
                        (333, 3), 
                        (444, 4), 
                        (555, 5), 
                        (888, 1);


# Queries and Operations
Various SQL queries are executed, including:
- Selection and retrieval of data.
- Updates to existing records.
- Deletion of records.
- Altering table structures.
- Complex joins and subqueries.

# Example Queries

# Select all customers
sql
SELECT * FROM customer;


# Update customer name
sql
UPDATE customer 
SET cname = 'rehaan' 
WHERE cid = 4;


# Complex Join Example
sql
SELECT a.accno, c.cname 
FROM account a, customer c, hold h 
WHERE a.accno = h.accno AND h.cid = c.cid;


# Views

Two views are created for simplified data access:

# XYZ View
sql
CREATE VIEW XYZ AS 
SELECT bid, bname 
FROM branch;

# ABC View
sql
CREATE VIEW abc AS 
SELECT c.cid, c.cname, a.acctype 
FROM customer c, account a, hold h 
WHERE a.accno = h.accno AND h.cid = c.cid;


# Triggers

A trigger is created to automatically update the account balance before an update operation:

# Balance Update Trigger
sql
CREATE TRIGGER blc 
BEFORE UPDATE ON account 
FOR EACH ROW 
SET NEW.accbalance = NEW.accbalance + NEW.transaction;


# How to Use

1. *Clone the repository*:
   bash
   git clone https://github.com/your-username/hdfc-bank-database.git
   cd hdfc-bank-database
   
2. *Set up the database*:
   sql
   mysql -u your_username -p < Database_Queries.txt

3. *Run individual queries* as needed using your preferred MySQL client.

4. In the repository you can find attached Database_Queries.txt file which contains all the queries.
