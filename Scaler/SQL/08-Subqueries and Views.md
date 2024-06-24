## SUBQUERIES

Imagine you are given an employee table which has all employee ID, names and their salary.
If I asked you to report employees who earn more than company average, how would you do it? 

Note that the following query does not work:

```sql
SELECT * FROM employees WHERE salary > AVG(salary)
```

**Why?:** Because where clause filters row by row. Aggregation only happens at the time of printing (unless for group_by and having clause). WHERE clause does not know about AVG(salary). 

Manually, I will first find out the company average and then put it in a query. But what if I did not want to put it manually. Subqueries come to our rescue here. 

If I break the above down, what are the steps:

 - Step 1: Find the average salary amongst all employees. 

```sql
SELECT AVG(salary) FROM employees
```

 - Step 2: Find all employees who make more than the above number. 

```sql
SELECT * FROM employees WHERE salary > (SELECT AVG(salary) FROM employees)
```

As you can see, in SQL, it's possible to place a SQL query inside another query. This inner query is known as a subquery.
In a subquery, the outer query's result depends on the result set of the inner subquery. That's why subqueries are also called nested queries.

### More examples

***Q: Find all employees who make more salary than their department average.***

What's step 1 here? I need to find out department wise average salaries. 
Ok, let's do that. 

```sql
SELECT dept, AVG(salary) FROM employees GROUP BY dept
```

If the above was a table, would it be possible to get result quickly by doing a JOIN. Oh yes, absolutely. Let's do that then. 

```sql
SELECT
  e.id, e.name
FROM employees AS e
  JOIN (SELECT dept, AVG(salary) AS dept_salary FROM employees GROUP BY dept) AS tmp_table
  ON (e.dept = tmp_table.dept AND e.salary > tmp_table.dept_salary)
```

Hope this helps build intuition about group by, having and sub-queries. 