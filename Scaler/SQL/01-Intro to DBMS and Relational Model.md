# Lecture 1: Introduction to DBMS and SQL

## Agenda

- What is a Database
- Types of Databases
- Intro to Relational Databases
- Intro to Keys

### What's a Database

 A database is nothing but a collection of related data. 

### What's a Database Management System (DBMS)

A DBMS as the name suggests is a software system that allows to efficiently manage a database. A DBMS allows us to create, retrieve, update, and delete data (often also called CRUD operations). It also  allows to define rules to ensure data integrity, security, and concurrency. It also provides ways to query the data in the database efficiently.

## Types of Databases

- Relational Databases
- Non-Relational Databases

### Relational Databases

Relational Databases allow you to represent a database as a collection of multiple related tables. Each table has a set of columns and rows. Each row represents a record and each column represents a field. 

1. Relational Databases represent a database as a collection of tables with each table storing information about something. This something can be an entity or a relationship between entities. 
2. Every row is unique. This means that in a table, no 2 rows can have same values for all columns. 
3. All of the values present in a column hold the same data type. 
4. Values are atomic. 
5. The columns sequence is not guaranteed. This is very important. SQL standard doesn't guarantee that the columns will be stored in the same sequence as you define them.

6. The rows sequence is not guaranteed. Similar to columns, SQL doesn't guarantee the order in which rows shall be returned after any query. So, if you want to get rows in a particular order, you should always use `ORDER BY` clause in your query .
7. The name of every column is unique. This means that in a table, no 2 columns can have same name.


### Non-Relational Databases

Now that we have learnt about relational databases, let's talk about non-relational databases. Non-relational databases are those databases that don't follow the relational model. They don't store data in form of tables. Instead, they store data in form of documents, key-value pairs, graphs, etc. 
