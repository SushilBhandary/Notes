
### Joins

A join condition is a condition that must be true between the rows of 2 tables for them to be stitched together. Let's see how we can write a join query for our example. 

Every SQL query we had written till now was only finding data from 1 table. Most of the queries we had written in the previous classes were on the `film` table where we applied multiple filters etc. But do you think being able to query data from a single table is enough? Let's take a scenario of Scaler. Let's say we have 2 tables as follows in the Scaler's database:

`batches`

| batch_id | batch_name |
|----------|------------|
| 1        | Batch A    |
| 2        | Batch B    |
| 3        | Batch C    |

`students`

| student_id | first_name | last_name | batch_id |
|------------|------------|-----------|----------|
| 1          | John       | Doe       | 1        |
| 2          | Jane       | Doe       | 1        |
| 3          | Jim        | Brown     | 2        |
| 4          | Jenny      | Smith     | 3        |
| 5          | Jack       | Johnson   | 2        |

Suppose, someone asks you to print the name of every student, along with the name of their batch. The output should be something like:

| student_name | batch_name |
|--------------|------------|
| John     | Batch A    |
| Jane     | Batch A    |
| Jim    | Batch B    |
| Jenny  | Batch C    |
| Jack | Batch B    |


We would want to stitch a row of students table with a row of batches table based on the `batch_id` column. This is what we call a `join condition`.

```sql
SELECT students.first_name, batches.batch_name
FROM students
JOIN batches
ON students.batch_id = batches.batch_id;
```

Let's take an example of this on the Sakila database. Let's say for every film, we want to print its name and the language. How can we do that?

```sql
SELECT film.title, language.name
FROM film
JOIN language
ON film.language_id = language.language_id;
```

Now, sometimes typing name of tables in the query can become difficult. For example, in the above query, we have to type `film` and `language` multiple times. To make this easier, we can give aliases to the tables. For example, we can give the alias `f` to the `film` table and `l` to the `language` table. We can then use these aliases in our query. Let's see how we can do that:

```sql
SELECT f.title, l.name
FROM film AS f
JOIN language AS l
ON f.language_id = l.language_id;
```
