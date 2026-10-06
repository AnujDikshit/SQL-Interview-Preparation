# SQL-Interview-Preparation
Industry-level SQL practice for Data Analyst interviews
-- 1. All employees
SELECT * FROM employees;

-- 2. Selected columns
SELECT name, salary FROM employees;

-- 3. Salary > 50K
SELECT * FROM employees WHERE salary > 50000;

-- 4. Salary = 50K
SELECT * FROM employees WHERE salary = 50000;

-- 5. Salary < 30K
SELECT * FROM employees WHERE salary < 30000;

-- 6. Employees from Lucknow
SELECT * FROM employees WHERE city = 'Lucknow';

-- 7. IT employees
SELECT * FROM employees WHERE department = 'IT';

-- 8. Salary between 30K–70K
SELECT * FROM employees WHERE salary BETWEEN 30000 AND 70000;

-- 9. IT or HR
SELECT * FROM employees WHERE department IN ('IT','HR');

-- 10. Not IT
SELECT * FROM employees WHERE department <> 'IT';

-- 11. Highest salary first
SELECT * FROM employees ORDER BY salary DESC;

-- 12. Lowest salary first
SELECT * FROM employees ORDER BY salary;

-- 13. Top 5 salaries
SELECT * FROM employees ORDER BY salary DESC LIMIT 5;

-- 14. Bottom 5 salaries
SELECT * FROM employees ORDER BY salary LIMIT 5;

-- 15. Name starts A
SELECT * FROM employees WHERE name LIKE 'A%';

-- 16. Name ends n
SELECT * FROM employees WHERE name LIKE '%n';

-- 17. Name contains an
SELECT * FROM employees WHERE name LIKE '%an%';

-- 18. Unique departments
SELECT DISTINCT department FROM employees;

-- 19. Unique cities
SELECT DISTINCT city FROM employees;

-- 20. Count employees
SELECT COUNT(*) FROM employees;

-- 21. Average salary
SELECT AVG(salary) FROM employees;

-- 22. Maximum salary
SELECT MAX(salary) FROM employees;

-- 23. Minimum salary
SELECT MIN(salary) FROM employees;

-- 24. Total salary
SELECT SUM(salary) FROM employees;

-- 25. Count IT employees
SELECT COUNT(*) FROM employees WHERE department='IT';

-- 26. Average IT salary
SELECT AVG(salary) FROM employees WHERE department='IT';

-- 27. Highest HR salary
SELECT MAX(salary) FROM employees WHERE department='HR';

-- 28. Lowest Finance salary
SELECT MIN(salary) FROM employees WHERE department='Finance';

-- 29. Unique city count
SELECT COUNT(DISTINCT city) FROM employees;

-- 30. Unique department count
SELECT COUNT(DISTINCT department) FROM employees;

-- 31. Employees per department
SELECT department,COUNT(*) FROM employees GROUP BY department;

-- 32. Average salary by department
SELECT department,AVG(salary) FROM employees GROUP BY department;

-- 33. Maximum salary by department
SELECT department,MAX(salary) FROM employees GROUP BY department;

-- 34. Minimum salary by department
SELECT department,MIN(salary) FROM employees GROUP BY department;

-- 35. Total salary by department
SELECT department,SUM(salary) FROM employees GROUP BY department;

-- 36. Employees per city
SELECT city,COUNT(*) FROM employees GROUP BY city;

-- 37. Departments >5 employees
SELECT department,COUNT(*) FROM employees
GROUP BY department HAVING COUNT(*)>5;

-- 38. Departments avg salary >60K
SELECT department,AVG(salary) FROM employees
GROUP BY department HAVING AVG(salary)>60000;

-- 39. Orders per status
SELECT status,COUNT(*) FROM orders GROUP BY status;

-- 40. Revenue per status
SELECT status,SUM(amount) FROM orders GROUP BY status;

-- 41. Customer + orders
SELECT c.name,o.order_id,o.amount
FROM customers c JOIN orders o ON c.customer_id=o.customer_id;

-- 42. All customers + orders
SELECT c.name,o.order_id,o.amount
FROM customers c LEFT JOIN orders o ON c.customer_id=o.customer_id;

-- 43. Customers with no orders
SELECT c.* FROM customers c
LEFT JOIN orders o ON c.customer_id=o.customer_id
WHERE o.order_id IS NULL;

-- 44. Employee + manager
SELECT e.name AS employee,m.name AS manager
FROM employees e LEFT JOIN employees m
ON e.manager_id=m.employee_id;

-- 45. Customer total spending
SELECT c.name,SUM(o.amount) AS spending
FROM customers c JOIN orders o ON c.customer_id=o.customer_id
GROUP BY c.customer_id,c.name;

-- 46. Customer order count
SELECT c.name,COUNT(o.order_id)
FROM customers c LEFT JOIN orders o
ON c.customer_id=o.customer_id
GROUP BY c.customer_id,c.name;

-- 47. Product + quantity
SELECT p.product_name,oi.quantity
FROM products p JOIN order_items oi
ON p.product_id=oi.product_id;

-- 48. Product revenue
SELECT p.product_name,SUM(oi.quantity*p.price)
FROM products p JOIN order_items oi
ON p.product_id=oi.product_id
GROUP BY p.product_id,p.product_name;

-- 49. Best-selling product
SELECT p.product_name,SUM(oi.quantity) qty
FROM products p JOIN order_items oi
ON p.product_id=oi.product_id
GROUP BY p.product_id,p.product_name
ORDER BY qty DESC LIMIT 1;

-- 50. Products never ordered
SELECT p.* FROM products p
LEFT JOIN order_items oi ON p.product_id=oi.product_id
WHERE oi.product_id IS NULL;

-- 51. Above average salary
SELECT * FROM employees
WHERE salary>(SELECT AVG(salary) FROM employees);

-- 52. Highest salary employee
SELECT * FROM employees
WHERE salary=(SELECT MAX(salary) FROM employees);

-- 53. Below average salary
SELECT * FROM employees
WHERE salary<(SELECT AVG(salary) FROM employees);

-- 54. Second highest salary
SELECT MAX(salary) FROM employees
WHERE salary<(SELECT MAX(salary) FROM employees);

-- 55. Third highest salary
SELECT MAX(salary) FROM employees
WHERE salary<(SELECT MAX(salary) FROM employees
WHERE salary<(SELECT MAX(salary) FROM employees));

-- 56. Customers with orders
SELECT * FROM customers c
WHERE EXISTS(SELECT 1 FROM orders o
WHERE o.customer_id=c.customer_id);

-- 57. Customers without orders
SELECT * FROM customers c
WHERE NOT EXISTS(SELECT 1 FROM orders o
WHERE o.customer_id=c.customer_id);

-- 58. Salary category
SELECT name,salary,
CASE WHEN salary>=80000 THEN 'High'
WHEN salary>=50000 THEN 'Medium'
ELSE 'Low' END AS category
FROM employees;

-- 59. Age category
SELECT name,age,
CASE WHEN age<25 THEN 'Young'
WHEN age<40 THEN 'Adult'
ELSE 'Senior' END
FROM customers;

-- 60. Hired after 2024
SELECT * FROM employees WHERE hire_date>'2024-01-01';

-- 61. Hired in 2024
SELECT * FROM employees
WHERE EXTRACT(YEAR FROM hire_date)=2024;

-- 62. Last 1 year hires
SELECT * FROM employees
WHERE hire_date>=CURRENT_DATE-INTERVAL '1 year';

-- 63. NULL managers
SELECT * FROM employees WHERE manager_id IS NULL;

-- 64. Replace NULL
SELECT name,COALESCE(manager_id,0) FROM employees;

-- 65. Uppercase names
SELECT UPPER(name) FROM employees;

-- 66. Name length
SELECT name,LENGTH(name) FROM employees;

-- 67. Rank salaries
SELECT name,salary,
RANK() OVER(ORDER BY salary DESC) AS rank
FROM employees;

-- 68. Rank within department
SELECT name,department,salary,
RANK() OVER(PARTITION BY department ORDER BY salary DESC)
FROM employees;

-- 69. Second highest using DENSE_RANK
SELECT * FROM(
SELECT name,salary,
DENSE_RANK() OVER(ORDER BY salary DESC) r
FROM employees)t WHERE r=2;

-- 70. Highest paid per department
SELECT * FROM(
SELECT name,department,salary,
RANK() OVER(PARTITION BY department ORDER BY salary DESC) r
FROM employees)t WHERE r=1;

-- 71. Top 3 per department
SELECT * FROM(
SELECT name,department,salary,
DENSE_RANK() OVER(PARTITION BY department ORDER BY salary DESC) r
FROM employees)t WHERE r<=3;

-- 72. Running revenue
SELECT order_id,order_date,amount,
SUM(amount) OVER(ORDER BY order_date) AS running_total
FROM orders;

-- 73. Revenue percentage
SELECT order_id,amount,
ROUND(amount*100.0/SUM(amount) OVER(),2)
FROM orders;

-- 74. Latest order per customer
SELECT * FROM(
SELECT o.*,ROW_NUMBER() OVER(
PARTITION BY customer_id ORDER BY order_date DESC) rn
FROM orders o)t WHERE rn=1;

-- 75. Duplicate emails
SELECT email,COUNT(*) FROM customers
GROUP BY email HAVING COUNT(*)>1;

-- 76. Top 3 customers
SELECT c.name,SUM(o.amount) spending
FROM customers c JOIN orders o
ON c.customer_id=o.customer_id
GROUP BY c.customer_id,c.name
ORDER BY spending DESC LIMIT 3;

-- 77. Orders > average order
SELECT * FROM orders
WHERE amount>(SELECT AVG(amount) FROM orders);

-- 78. Customers spending >100K
SELECT customer_id,SUM(amount)
FROM orders GROUP BY customer_id
HAVING SUM(amount)>100000;

-- 79. Department with highest average salary
SELECT department,AVG(salary) avg_salary
FROM employees GROUP BY department
ORDER BY avg_salary DESC LIMIT 1;

-- 80. City with most employees
SELECT city,COUNT(*) total
FROM employees GROUP BY city
ORDER BY total DESC LIMIT 1;

-- 81. Total quantity by category
SELECT p.category,SUM(oi.quantity)
FROM products p JOIN order_items oi
ON p.product_id=oi.product_id
GROUP BY p.category;

-- 82. Revenue by category
SELECT p.category,SUM(oi.quantity*p.price)
FROM products p JOIN order_items oi
ON p.product_id=oi.product_id
GROUP BY p.category;

-- 83. Average product price
SELECT AVG(price) FROM products;

-- 84. Products above average price
SELECT * FROM products
WHERE price>(SELECT AVG(price) FROM products);

-- 85. Products with low stock
SELECT * FROM products WHERE stock<10;

-- 86. Most expensive product
SELECT * FROM products ORDER BY price DESC LIMIT 1;

-- 87. Cheapest product
SELECT * FROM products ORDER BY price LIMIT 1;

-- 88. Completed orders
SELECT * FROM orders WHERE status='Completed';

-- 89. Revenue from completed orders
SELECT SUM(amount) FROM orders WHERE status='Completed';

-- 90. Monthly revenue
SELECT EXTRACT(MONTH FROM order_date) month,
SUM(amount) revenue
FROM orders GROUP BY month ORDER BY month;

-- 91. Daily revenue
SELECT order_date,SUM(amount)
FROM orders GROUP BY order_date ORDER BY order_date;

-- 92. Orders above 10K
SELECT * FROM orders WHERE amount>10000;

-- 93. Customer's highest order
SELECT customer_id,MAX(amount)
FROM orders GROUP BY customer_id;

-- 94. Customer's average order
SELECT customer_id,AVG(amount)
FROM orders GROUP BY customer_id;

-- 95. Customer's first order
SELECT customer_id,MIN(order_date)
FROM orders GROUP BY customer_id;

-- 96. Customer's latest order date
SELECT customer_id,MAX(order_date)
FROM orders GROUP BY customer_id;

-- 97. Employees without managers
SELECT name FROM employees WHERE manager_id IS NULL;

-- 98. Employees earning > department average
SELECT * FROM employees e
WHERE salary>(
SELECT AVG(salary) FROM employees
WHERE department=e.department);

-- 99. Highest salary in each city
SELECT * FROM(
SELECT name,city,salary,
RANK() OVER(PARTITION BY city ORDER BY salary DESC) r
FROM employees)t WHERE r=1;

-- 100. Top customer by spending
SELECT c.name,SUM(o.amount) spending
FROM customers c JOIN orders o
ON c.customer_id=o.customer_id
GROUP BY c.customer_id,c.name
ORDER BY spending DESC LIMIT 1;
