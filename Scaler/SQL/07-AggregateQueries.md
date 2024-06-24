## Agenda

 - Group By
 - Group by on multiple columns
 - Having clause

## GROUP BY Clause

The way Group By clause works is it allows us to break the table into multiple groups so as to be used by the aggregate function. 

For example: `GROUP BY batch_id` will bring all rows with same `batch_id` together in one group

> Note: Also, GROUP BY always works before aggregate functions. Group By is used to apply aggregate function within groups (collection of rows). The result comes out to be a set of values where each value is derived from its corresponding group.

Let's take an example.


| id | name | age | batch_id |
|----|------|-----|----------|
| 1  | A    | 20  | 1        |
| 2  | B    | 21  | 3        |
| 3  | C    | 22  | 1        |
| 4  | D    | 23  | 2        |
| 5  | E    | 23  | 1        |
| 6  | F    | 25  | 2        |
| 7  | G    | 22  | 3        |
| 8  | H    | 21  | 2        |
| 9  | I    | 20  | 1        |

```sql
SELECT COUNT(*), batch_id FROM students GROUP BY batch_id;
```

The result of above query will be:
| COUNT(\*) | batch_id |
|-----------|----------|
| 4         | 1        |
| 3         | 2        |
| 2         | 3        |

Explanation: The query breaks the table into 3 groups each having rows with `batch_id` as 1, 2, 3 respectively. There are 4 rows with `batch_id = 1`, 3 rows with `batch_id = 2` and 2 rows with `batch_id = 3`.

Note that, we can only use the columns in SELECT which are present in Group By because only those columns will have same value across all rows in a group.

**Ordering of Operations:**

```
1. Select the tables. 
  1.2. Do the joins if required
2. Apply the filters (where clause) 
3. Group by 
   3.2: Filtering happens with having clause 
4. Select what to print including aggregates
5. Order by, limit, offset. 
```

### Examples

***Q1: Print the name of every actor (first_name, last_name) and with that print the number of films they have acted in.***

```sql
SELECT
    a.first_name, a.last_name, count(1) AS count_movies

  FROM actor a
    JOIN film_actor b
       ON a.actor_id = b.actor_id
  
       
GROUP BY a.actor_id
```

***Q2: If you have a table `user_classes` with columns as (userID, classID, attended (0 / 1)), then find user-wise attendance.***

```sql
SELECT userid, AVG(attended) FROM user_classes GROUP BY userid
```

### Group by on multiple columns

**Table: companies_users**

| user_id | company_id | round_date | interview_result |
| --- | --- | --- | --- |
| 1 | 1 | 2023-07-01 | 1 |
| 1 | 1 | 2023-07-03 | 1 |
| 1 | 1 | 2023-07-07 | 0 |
| 1 | 2 | 2023-07-02 | 0 |
| 2 | 2 | 2023-07-03 | 1 |
| 2 | 2 | 2023-07-04 | 1 |

***Q:Find the number of rounds given by every user in every company they interviewed for.***

```sql
SELECT user_id, company_id, count(1)
FROM companies_users 
GROUP BY user_id, company_id
```

## HAVING Clause

HAVING clause is used to filter groups. Let's take a question to understand the need of HAVING clause:

There are 2 tables: Students(id, name, age, batch_id) and Batches(id, name). Print the batch names that have more than 100 students along with count of the students in each batch.

```sql
SELECT COUNT(S.id), B.name
FROM Students S
JOIN Batches B ON S.batch_id = B.id
GROUP BY B.name;
HAVING COUNT(S.id) > 100;
```

Here, `GROUP BY B.name` groups the results by the `B.name` column (batch name). It ensures that the count is calculated for each distinct batch name. 
`HAVING COUNT(S.id) > 100` condition filters the grouped results based on the count of `S.id` (number of students). It retains only the groups where the count is greater than 100.

The sequence in which query executes is: 
- Firstly, join of the two tables is done. 
- Then is is divided into groups based on `B.name`. 
- In the third step, result is filtered using the condition in HAVING clause. 
- Lastly, it is printed through SELECT.

FROM -> WHERE -> GROUP BY -> HAVING -> SELECT 

WHERE is not build to be able to handle aggregates. We can not use WHERE after GROUP BY because WHERE clause works on rows and as soon as GROUP BY forms a result, the rows are convereted into groups. So, no individual conditions or actions can be performed on rows after GROUP BY. 

