# SQL
-- Select second highest salary in table:-

   Select Distinct Salary
   from emplyoees
   Order By Salary DESC
   LIMIT 1 OFFSET 1;

-- Department having employee count more than 5:-

  Select departmentid, COUNT(*) As emp_count
  from employees
  GROUP BY department_id
  HAVING count(*) > 5;

-- Employees earning more than their managers:-

  Select e.name AS employee_name, e.salary, m.name AS manager_name, m.salary AS manager_salary
  from employees e
  join employee m ON e.manager_id = m.emp_id
  Where e.salary > m.salary;

-- Top 3 performing products based on sales:-

  Select product_id, product_name, total_sales 
  from sales_data
  ORDER BY total_sales DESC
  LIMIT 3;

-- Calculate cumulative sum of sales 

  Select order_date, product_id, sales_amount
  SUM(sales_amount) 
  OVER(PARTITION BY product_id ORDER BY order_date) 
  AS cumulatibe_sales from sales;

-- Customer who made transaction above Rs 5000 multiple times

  Select customer_id, COUNT(*) AS high_value_txns
  from transactions
  where transaction_amount > 5000
  GROUP BY customer_id
  HAVING COUNT(*) > 1;

-- customer haven't made any purchases in last 6 months

  Select c.cutomer_id, c.name
  from customer c
  Left Join transactions t 
  ON c.customer_id = t.customer_id
  AND t.transaction_date >= Current_DATE - INTERVAL '6 months'
  where t.customer_id IS NULL;

-- maximum transaction amount for each customer

  Select customer_id, MAX(amount) AS max_transaction
  fr0m transctions
  GROUP BY customer_id;

-- employee names along with their manager names

  Select e.name As employee_name, m.name AS manager_name
  from employee e 
  join employee m ON e.manager_id = m.emp_id

-- most profitable regions based on transaction data

   Select regions, SUM(amount) As total_revenue
   from transactions
   GROUP BY region
   ORDER BY total_revenue DESC
   LIMIT 3;

-- Calculate age from DOB

   Select name 
   FLOOR(DATEDIFF(CURDATE(), DOB)/365) AS Age
   from employeee;

   Select Name, 
   YEAR(CURDATE()) - YEAR(DOB) As Age
   from Employees;

-- Duplicate emails in Employee Table

   Select Email, COUNT(Email) As DuplicateCount
   from Employees
   GROUP BY Email
   HAVING COUNT(Email) > 1;

-- Highest Salary in each department

   Select Department, MAX(Salary) AS HighestSalary
   from Empolyees
   GROUP BY Department;

-- Employees joined in last 3 months

   Select Name, JoiningDate
   from Employees 
   where DATEDIFF(CURDATE(), JoiningDate) <= 90;

-- First 5 records in SQL

   Select * from Employees
   LIMIT 5;

   Select Top 5 * from Employees

-- Number of Employees in each department 

   Select department, COUNT(EmpId) AS EmployeeCount
   from Employees
   GROUP BY Department

-- Last 3 records in SQL

   Select * from employees
   ORDER BY EmpID DESC
   LIMIT 3;

--  Employee without manager

   Select Name, EmpId
   from Employees
   WHERE managerId IS NULL

--  First name with 'A'

   SELECT name from employees 
   where Name LIKE 'A%'

--  Display duplicate records with count

   Select columnName, COUNT(coloumnName) AS count
   from Tablename
   GROUP BY columnName
   Having count(columnName) >1;

-- Department with highest employee count

   Select department COUNT(EmpId) AS EmployeeCount 
   from Employees
   Group BY depatment 
   ORDER BY EmployeeCount DESC 
   LIMIT 1;

-- Find duplicate employee names

   SELECT name, COUNT(*) AS count
   FROM employees
   GROUP BY name
   HAVING COUNT(*) > 1;
   
-- Find all employees who joined in the last 6 months

   SELECT *
   FROM employees
   WHERE join_date >= DATE_SUB(CURDATE(), INTERVAL 6 MONTH);

-- Get department-wise average salary

   SELECT department_id, AVG(salary) AS avg_salary
   FROM employees
   GROUP BY department_id;

-- Get top 3 highest-paid employees per department

   SELECT e1.*
   FROM employees e1
   WHERE (
     SELECT COUNT(DISTINCT e2.salary)
     FROM employees e2
     WHERE e2.department_id = e1.department_id
       AND e2.salary > e1.salary
   ) < 3;

-- Find employees working in more than one project

   SELECT employee_id, COUNT(DISTINCT project_id) AS project_count
   FROM employee_project
   GROUP BY employee_id
   HAVING COUNT(DISTINCT project_id) > 1;

-- Find all employees without any assigned project

   SELECT e.*
   FROM employees e
   LEFT JOIN employee_project ep ON e.id = ep.employee_id
   WHERE ep.project_id IS NULL;   

-- Get employees whose salary is between 50K and 80K

   SELECT * FROM employees
   WHERE salary BETWEEN 50000 AND 80000;

-- Find employees joined before their manager

   SELECT e.name AS employee, m.name AS manager
   FROM employees e
   JOIN employees m ON e.manager_id = m.id
   WHERE e.join_date < m.join_date;


Assume you have these tables:

employees

emp_id	emp_name	dept_id	manager_id	salary
1	      Adam	   10	        NULL	   90000
2	      John	   10	         1	      75000
3	      Mary	   20	         1	      65000
4	      Steve	   20	         3	      60000
5	      Raj	   30	         2	      55000

departments

dept_id	dept_name
10	      IT
20	      HR
30	      Finance

projects

proj_id	proj_name	dept_id
101	     WebApp	      10
102	     Payroll	   20
103	     Audit	      30

employee_project

emp_id	proj_id
1	        101
2	        101
3	        102
4	        102
5	        103


🔹 1️⃣ List employee names with their department names
SELECT e.emp_name, d.dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;

🔹 2️⃣ List all employees and their project names
SELECT e.emp_name, p.proj_name
FROM employees e
JOIN employee_project ep ON e.emp_id = ep.emp_id
JOIN projects p ON ep.proj_id = p.proj_id;

🔹 3️⃣ List employees who are not assigned to any project
SELECT e.emp_name
FROM employees e
LEFT JOIN employee_project ep ON e.emp_id = ep.emp_id
WHERE ep.proj_id IS NULL;

🔹 4️⃣ Find all departments that have no employees
SELECT d.dept_name
FROM departments d
LEFT JOIN employees e ON d.dept_id = e.dept_id
WHERE e.emp_id IS NULL;

🔹 5️⃣ Get employee details along with their manager’s name

(Self Join)

SELECT e.emp_name AS Employee, m.emp_name AS Manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.emp_id;

🔹 6️⃣ Find employees working in the same department as ‘Adam’
SELECT e.emp_name
FROM employees e
WHERE e.dept_id = (
    SELECT dept_id FROM employees WHERE emp_name = 'Adam'
)
AND e.emp_name <> 'Adam';

🔹 7️⃣ List departments with total salary expenditure
SELECT d.dept_name, SUM(e.salary) AS total_salary
FROM departments d
JOIN employees e ON d.dept_id = e.dept_id
GROUP BY d.dept_name;

🔹 8️⃣ Get employees who work in departments having more than 1 project
SELECT DISTINCT e.emp_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
JOIN projects p ON d.dept_id = p.dept_id
WHERE d.dept_id IN (
    SELECT dept_id FROM projects GROUP BY dept_id HAVING COUNT(*) > 1
);

🔹 9️⃣ List project names and count of employees in each project
SELECT p.proj_name, COUNT(ep.emp_id) AS employee_count
FROM projects p
LEFT JOIN employee_project ep ON p.proj_id = ep.proj_id
GROUP BY p.proj_name;

🔹 🔟 Show employees and their manager names from same department only
SELECT e.emp_name AS Employee, m.emp_name AS Manager
FROM employees e
JOIN employees m ON e.manager_id = m.emp_id
WHERE e.dept_id = m.dept_id;

🔹 11️⃣ List employees who work on projects outside their department
SELECT e.emp_name, p.proj_name
FROM employees e
JOIN employee_project ep ON e.emp_id = ep.emp_id
JOIN projects p ON ep.proj_id = p.proj_id
WHERE e.dept_id <> p.dept_id;

🔹 12️⃣ Find the highest-paid employee in each department
SELECT e.emp_name, e.salary, d.dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id
WHERE e.salary = (
    SELECT MAX(salary)
    FROM employees
    WHERE dept_id = d.dept_id
);

🔹 13️⃣ Find employees who share the same manager
SELECT e1.emp_name AS Emp1, e2.emp_name AS Emp2, e1.manager_id
FROM employees e1
JOIN employees e2 ON e1.manager_id = e2.manager_id
WHERE e1.emp_id <> e2.emp_id;

🔹 14️⃣ Get department-wise number of employees working on projects
SELECT d.dept_name, COUNT(DISTINCT e.emp_id) AS emp_count
FROM departments d
JOIN employees e ON d.dept_id = e.dept_id
JOIN employee_project ep ON e.emp_id = ep.emp_id
GROUP BY d.dept_name;

🔹 15️⃣ Get all projects with department name and employees working on them
SELECT p.proj_name, d.dept_name, e.emp_name
FROM projects p
JOIN departments d ON p.dept_id = d.dept_id
JOIN employee_project ep ON p.proj_id = ep.proj_id
JOIN employees e ON ep.emp_id = e.emp_id;

   
   
   


  
  
  












  
