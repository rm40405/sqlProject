-- Create table for employees
CREATE DATABASE emp;
CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    department_id INT,
    hire_date DATE,
    position VARCHAR(100)
);

-- Create table for departments
CREATE DATABASE depart;

CREATE TABLE departments (
    department_id INT PRIMARY KEY AUTO_INCREMENT,
    department_name VARCHAR(100) NOT NULL
);

-- Create table for performance reviews
CREATE DATABASE perre;
CREATE TABLE performance_reviews (
    review_id INT PRIMARY KEY AUTO_INCREMENT,
    employee_id INT,
    review_date DATE NOT NULL,
    sales_performance DECIMAL(5, 2),
    customer_feedback DECIMAL(5, 2),
    project_completion DECIMAL(5, 2)
   
);
-- Insert data into departments
INSERT INTO departments (department_name) VALUES 
('Sales'), 
('Marketing'), 
('HR'), 
('IT');


-- Insert data into employees
INSERT INTO employees (name, department_id, hire_date, position) VALUES 
('Alice', 1, '2020-05-20', 'Sales Manager'), 
('Bob', 2, '2019-04-15', 'Marketing Specialist'), 
('Charlie', 1, '2021-03-10', 'Sales Representative'), 
('Diana', 3, '2022-06-30', 'HR Manager'), 
('Evan', 4, '2020-08-05', 'IT Support');

-- Insert data into performance_reviews
INSERT INTO performance_reviews (employee_id, review_date, sales_performance, customer_feedback, project_completion) VALUES 
(1, '2023-07-01', 90.5, 85.0, 92.0),
(2, '2023-07-01', 75.0, 90.0, 88.0),
(3, '2023-07-01', 60.5, 70.0, 80.0),
(1, '2023-06-01', 88.0, 82.0, 90.0),
(4, '2023-07-01', NULL, 95.0, 85.0),
(5, '2023-07-01', NULL, 87.0, 95.0);

SELECT COUNT(*) AS total_reviews
FROM performance_reviews;

SELECT AVG(sales_performance) AS average_sales_performance
FROM performance_reviews
WHERE sales_performance IS NOT NULL;

SELECT MAX(customer_feedback) AS highest_customer_feedback
FROM performance_reviews;

SELECT d.department_name, SUM(pr.project_completion) AS total_project_completion
FROM performance_reviews pr
JOIN employees e ON pr.employee_id = e.employee_id
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name;

SELECT d.department_name, 
       AVG(pr.sales_performance) AS average_sales_performance, 
       AVG(pr.customer_feedback) AS average_customer_feedback, 
       AVG(pr.project_completion) AS average_project_completion
FROM performance_reviews pr
JOIN employees e ON pr.employee_id = e.employee_id
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name;


SELECT d.department_name
FROM performance_reviews pr
JOIN employees e ON pr.employee_id = e.employee_id
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name
HAVING AVG(pr.sales_performance) > 80;

SELECT COUNT(DISTINCT review_date) AS distinct_review_dates
FROM performance_reviews;

SELECT e.name, COUNT(pr.review_id) AS total_reviews
FROM employees e
JOIN performance_reviews pr ON e.employee_id = pr.employee_id
GROUP BY e.name;

SELECT d.department_name, 
       AVG(pr.sales_performance) AS average_sales_performance, 
       COUNT(pr.review_id) AS total_reviews
FROM performance_reviews pr
JOIN employees e ON pr.employee_id = e.employee_id
JOIN departments d ON e.department_id = d.department_id
GROUP BY d.department_name;
