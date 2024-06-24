# Lecture 1: Keys

## Keys in Relational Databases

 Keys are used to uniquely identify a row in a table. There are 2 important types of keys: Primary Key and Foreign Key. There are also other types of keys like Super Key, Candidate Key etc. Let's learn about them one by one.

## Types of Keys

- Primary Key
- Foreign Key
- Super Key
- Candidate Key 
- Composite Key

### Super Keys

A `super key` is a combination of columns whose values can uniquely identify a row in a table. 

### Candidate keys

A `candidate key` is a super key from which no column can be removed and still have the property of uniquely identifying a row. If any more column is removed from a candidate key, it will no longer be able to uniquely identify a row.

### Composite Key

A `Composite Key` is having more that one coloum and uniquely identify a row.

### Primary Key

A `primary key` is a candidate key that is chosen to be the key for the table. 

```
CREATE TABLE Persons (
    ID int NOT NULL,
    LastName varchar(255) NOT NULL,
    FirstName varchar(255),
    Age int,
    PRIMARY KEY (ID)
); 
```

### Foreign Keys

 A `foreign key` is a column in a table that references a column in another table. It has nothing to do with primary, candidate, super keys. It can be any column in 1 table that refers to any column in other table. 

 ``` 
 CREATE TABLE Orders (
    OrderID int NOT NULL,
    OrderNumber int NOT NULL,
    PersonID int,
    PRIMARY KEY (OrderID),
    FOREIGN KEY (PersonID) REFERENCES Persons(PersonID)
);
```

| batch_id | batch_name |
|----------|------------|
| 1        | Batch A    |
| 2        | Batch B    |
| 3        | Batch C    |


| student_id | first_name | last_name | batch_id |
|------------|------------|-----------|----------|
| 1          | John       | Doe       | 1        |
| 2          | Jane       | Doe       | 1        |
| 3          | Jim        | Brown     | 2        |
| 4          | Jenny      | Smith     | 3        |
| 5          | Jack       | Johnson   | 2        |

Now let's say we `delete` the row with batch_id 2 from batches table. What will happen? Yes, the students Jim and Jack will be orphaned. They will be in the students table but there will be no batch with id 2. This is called orphaning. This is one of the problems with foreign keys. Another problem is that if we `update` the batch_id of a batch in batches table, it will not be updated in students table. Eg: If we update the batch_id of Batch A from 1 to 4, the students John and Jane will still have batch_id as 1. This is called inconsistency.

To fix for these, MySQL allows you to set ON DELETE constraints so that `cascading` deletes happen when such a delete happens.
1. `cascading` 
    -   if it `delete` from foreign key Table then It will aslo `delete` the row in Main Table.
    -   if it `update` from foreign key Table then It will aslo `update` the row in Main Table.
2. `Set NULL`
    - On deleting or Updating the foreign key Table value it set it Main Table to Null.
3. `NO Action`
    - On deleting or Updating the foreign key Table value it will do nothing. 
4. `Set Default`
    - On deleting or Updating the foreign key Table value it set it Main Table to Some default value.
