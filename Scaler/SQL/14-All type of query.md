

### WHERE
```
SELECT Distinct customer_id from customers where revenue > 0 and year = 2021 
```
### ALTER Table 
````
ALTER TABLE employees 
ADD New_salary int 
AS (ROUND( salary+(salary*0.2) ));
````

### ORDER BY
```
SELECT orderNumber, productCode, Round(quantityOrdered * priceEach,2) as  sub_total from orderdetails
Order By orderNumber, sub_total desc
```

### LIMIT
```
SELECT original_title, tagline, director FROM movies
order by popularity desc limit 5
```

### LIKE
```
select patient_id from patients 
where conditions Like "DIAB1%"
or conditions Like "% DIAB1%"
```

### BETWEEN
```
select original_title, genres, vote_average, revenue
from movies
where release_year between 2012 and 2015
order by original_title
```

### JOIN
```
select e.employee_id, e.first_name, e.last_name
from employees e
join employees m on e.manager_id = m.employee_id  
where e.hire_date < m.hire_date
order by e.employee_id
```

### CONCAT STRING
```
SELECT distinct e.manager_id, CONCAT(m.first_name,' ',m.last_name) as Manager, 
CONCAT(e.first_name,' ',e.last_name) as Employee
from employees e
join employees m on e.manager_id = m.employee_id
order by e.manager_id, Employee
```

### DATE_ADD
```
SELECT employee_id, first_name, last_name, salary, d.department_name, hire_date, l.city
from employees e
join departments d on d.department_id = e.department_id
join locations l on d.location_id = l.location_id
where hire_date between "1998-01-01" and DATE_ADD("1998-01-01", INTERVAL 90 DAY)
order by employee_id
```




### CROSS JOIN
```
SELECT
e.employeeNumber, e.firstName, e.lastName, o.officeCode, o.city
from employees e
cross join offices o
order by e.employeeNumber, o.officeCode
```
### LEFT JOIN
where q.id is not equal to n.id the null value will be filled
```
SELECT
q.id, q.year, ifnull( n.npv,0 ) as npv
from queries q
left join npv n on q.id=n.id and q.year=n.year 
order by id, year
date
```
### GROP BY
```
SELECT 
department_id, avg(salary) as avg_employee_salary
from employees
group by department_id
order by avg_employee_salary desc, department_id
```
### HAVING
```
SELECT
class
from courses
group by class
having count(*)>= 5
order by class desc
```



### COUNTING IN GROUP BY 
```
SELECT
c.customer_id, c.customer_name
FROM orders o
join customers c on c.customer_id = o.customer_id
group by c.customer_id, c.customer_name
having sum(o.product_name="Bread") >0 and 
sum(o.product_name="Milk") > 0 and 
sum(o.product_name="Eggs") = 0
order by customer_name
```

### SUB QUERY`
```
SELECT *
from employees
where employee_id not in (
    SELECT employee_id from job_history
)
order by employee_id
```


```
SELECT concat(first_name, " ", last_name) as full_name
from employees
where employee_id in (
    SELECT manager_id
    from employees
    group by manager_id
    having count(*)>=4
)
order by full_name
```































