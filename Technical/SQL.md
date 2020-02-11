# SQL

## Glossary
__SQL__ - Structured Query Language 

__Database__ - repository of data with basic functionality like adding, modifying and quering.

__RDBMS__ = Relational Database management system eg. MySQL, DB Warehouse on Cloud

__Relation__ - a table

__Atributes__ - columns in the table

__Tuple__ - rows in the table

__Cava__

__Degree__ - number of atributes in the table

__Primary key__ - usually autoincremented integer, a unique set of values that allow for a quick finding a correct row

__Logical key__ - also unique, possible to get rows with this key, it is more user-friendly e.g. string

__Foreing key__ - a key that points to another table's primary key

__Schema__ - a first row of the table with the names of the atributes

__DDL__ - Data Definition Language

__DML__ - Data Manipulation Language

## Data types
- CHAR(<number>) - string of a length <number>
- VARCHAR(<number>) - a varied length string (max length is <number>)
- INTEGER
- FLOAT
- TEXT
- BYTE(<number>)
- BLOB - images: binary large object
- DATE
- TIME
- TIMESTAMP

## Simple commands
### CREATE
CREATE DATABASE

CREATE TABLE <table_name>(  
  <atribute_name> <atribute_type> <additional_conditions>  
  <atribute_name> <atribute_type>  
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
  
 SELECT * FROM <table_name> WHERE <condition1> AND <condition2> GROUPED By <atribute> ORDERED BY <atribute>
  
 SELECT * FROM <table_name> WHERE <condition1> AND <condition2> ORDERED BY <atribute> (DESC -> descending)
  
 SELECT * FROM <table_name> WHERE <condition1> AND <condition2> ORDERED BY <atribute> LIMIT <max_number_of columns>
  
 SELECT * FROM <table1_name> JOIN <table2_name> WHERE <table1_foreing_key> == <table2_primary_key> 
 
 ### UPDATE
 UPDATE <table_name> SET <atribute>=<value> WHERE <condition>
  
 ## Functions
 COUNT() 
 MIN() 
 MAX() 
 AVG() 

## Key words
DESC - descending 
DISINCT - only unique values  
