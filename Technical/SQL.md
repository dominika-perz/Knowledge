# SQL

## Glossary
SQL - Structured Query Language 

Database - repository of data with basic functionality like adding, modifying and quering.

RDBMS = Relational Database management system eg. MySQL, DB Warehouse on Cloud

Relation - a table
Atributes - columns in the table
Tuple - rows in the table
Cava
Degree - number of atributes in the table
Primary key - usually autoincremented integer, a unique set of values that allow for a quick finding a correct row
Logical key - also unique, possible to get rows with this key, it is more user-friendly e.g. string
Foreing key - a key that points to another table's primary key
Schema - a first row of the table with the names of the atributes
DDL - Data Definition Language
DML - Data Manipulation Language

## Data types
CHAR(<number>) - string of a length <number>
VARCHAR(<number>) - a varied length string (max length is <number>)
INTEGER
FLOAT
TEXT
BYTE(<number>)
BLOB - images: binary large object
DATE
TIME
TIMESTAMP

## Simple commands
### CREATE
CREATE DATABASE

CREATE TABLE <table_name>( \\
  <atribute_name> <atribute_type> <additional_conditions> \\
  <atribute_name> <atribute_type> <\br>
  ...
  );
  
 additional_conditions:
 - AUTOINCREMENT
 - NOT NULL
 - PRIMARY KEY
 
 ### DELETE
 DELETE FROM <table_name> WHERE <condition>
 
 ### INSERT
 INSERT INTO <table_name>(<atributes_name>) VALUES (<values>)
 
 ### SELECT
 SELECT * or <function> or <atributes_name> FROM <table_name> -> * = all
  
 SELECT * FROM <table_name> WHERE <condition1> AND <condition2> 
  
 SELECT * FROM <table_name> WHERE <condition1> AND <condition2> ORDERED BY <atribute> (DESC -> descending)
  
 SELECT * FROM <table_name> WHERE <condition1> AND <condition2> ORDERED BY <atribute> LIMIT <max_number_of columns>
  
 SELECT * FROM <table1_name> JOIN <table2_name> WHERE <table1_foreing_key> == <table2_primary_key> 
 
 ### UPDATE
 UPDATE <table_name> SET <atribute>=<value> WHERE <condition>
