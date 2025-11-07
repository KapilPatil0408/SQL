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



  
  
  












  
