-- Create tables
CREATE TABLE departments (
	dept_no VARCHAR(4) NOT NULL,
	dept_name VARCHAR(40) NOT NULL,
	PRIMARY KEY (dept_no),
	UNIQUE (dept_name)
);

CREATE TABLE employees (
	emp_no INT NOT NULL,
	birth_date DATE NOT NULL,
	first_name VARCHAR NOT NULL,
	last_name VARCHAR NOT NULL,
	gender VARCHAR NOT NULL,
	hire_date DATE NOT NULL,
	PRIMARY KEY (emp_no)
);

CREATE TABLE dept_manager (
	dept_no VARCHAR(4) NOT NULL,
	emp_no INT NOT NULL,
	from_date DATE NOT NULL,
	to_date DATE NOT NULL,
	FOREIGN KEY (emp_no) REFERENCES employees (emp_no),
	FOREIGN KEY (dept_no) REFERENCES departments (dept_no),
	PRIMARY KEY (emp_no, dept_no)
);

CREATE TABLE salaries (
	emp_no INT NOT NULL,
	salary INT NOT NULL,
	from_date DATE NOT NULL,
	to_date DATE NOT NULL,
	FOREIGN KEY (emp_no) REFERENCES employees (emp_no),
	PRIMARY KEY (emp_no)
);

CREATE TABLE titles (
	emp_no INT NOT NULL,
	title VARCHAR NOT NULL,
	from_date DATE NOT NULL,
	to_date DATE NOT NULL,
	FOREIGN KEY (emp_no) REFERENCES employees (emp_no),
	PRIMARY KEY (emp_no, title, from_date)
);
	
CREATE TABLE dept_emp(
	emp_no INT NOT NULL,
	dept_no VARCHAR(4) NOT NULL,
	from_date DATE NOT NULL,
	to_date DATE NOT NULL,
	FOREIGN KEY (emp_no) REFERENCES employees (emp_no),
	FOREIGN KEY (dept_no) REFERENCES departments (dept_no),
	PRIMARY KEY (emp_no, dept_no)
);
--

--Deliverable 1: Retirement Titles


--1. Create a table that combines employee info and title info
SELECT employees.first_name,
	employees.last_name,
	employees.emp_no,
	titles.title,
	titles.from_date,
	titles.to_date
INTO retirement_titles
FROM employees
LEFT JOIN titles
ON employees.emp_no = titles.emp_no
WHERE (birth_date BETWEEN '1952-01-01' AND '1955-12-31')
AND (hire_date BETWEEN '1985-01-01' AND '1988-12-31');

--2 Use Dictinct with Orderby to remove duplicate rows
SELECT DISTINCT ON (emp_no) emp_no,
first_name,
last_name,
title
INTO unique_titles
FROM retirement_titles
WHERE retirement_titles.to_date = ('9999-01-01')
ORDER BY emp_no ASC;

--3 Write another query to retrieve the number of employees by their most recent job title who are about to retire.
SELECT COUNT(unique_titles.title),unique_titles.title
INTO retiring_titles
FROM unique_titles
GROUP BY unique_titles.title
ORDER BY unique_titles.title ASC;

-- This would be more useful if we had departments as well
--Suggested step 1
SELECT employees.first_name,
	employees.last_name,
	employees.emp_no,
	titles.title,
	titles.from_date,
	titles.to_date,
	dept_emp.dept_no
INTO retirement_titles_dept
FROM employees
LEFT JOIN titles
ON employees.emp_no = titles.emp_no
LEFT JOIN dept_emp
ON titles.emp_no = dept_emp.emp_no
WHERE (birth_date BETWEEN '1952-01-01' AND '1955-12-31')
AND (hire_date BETWEEN '1985-01-01' AND '1988-12-31');

--Suggested step 2
SELECT DISTINCT ON (emp_no) emp_no,
first_name,
last_name,
title,
dept_no
INTO unique_titles_dept
FROM retirement_titles_dept
WHERE retirement_titles_dept.to_date = ('9999-01-01')
ORDER BY emp_no ASC;

--Suggested step 3
SELECT COUNT (unique_titles_dept.title), unique_titles_dept.title, unique_titles_dept.dept_no
INTO retiring_titles_dept
FROM unique_titles_dept
GROUP BY unique_titles_dept.dept_no, unique_titles_dept.title
ORDER BY unique_titles_dept.dept_no ASC;

--Deliverable 2: Mentorship Program

--1. Create a table that combines employee info, start and end date, and title info
SELECT employees.first_name,
	employees.last_name,
	employees.emp_no,
	employees.birth_date,
	dept_emp.from_date,
	dept_emp.to_date,
	titles.title
INTO mentorship_eligibilty
FROM employees
LEFT JOIN dept_emp
ON employees.emp_no = dept_emp.emp_no
LEFT JOIN titles
ON dept_emp.emp_no = titles.emp_no
WHERE (birth_date BETWEEN '1965-01-01' AND '1965-12-31');

-- This would be more useful if we had departments as well
--Suggested step 1
SELECT departments.dept_name,
	departments.dept_no,
	dept_emp.from_date,
	dept_emp.to_date,
	dept_emp.emp_no
INTO dept_title
FROM departments
LEFT JOIN dept_emp
ON departments.dept_no = dept_emp.dept_no;

--Suggested step 2
SELECT dept_title.dept_name,
	dept_title.dept_no,
	dept_title.from_date,
	dept_title.to_date,
	dept_title.emp_no,
	employees.first_name,
	employees.last_name,
	employees.birth_date,
	titles.title
INTO mentorship_eligibilty_dept
FROM dept_title
LEFT JOIN employees
ON dept_title.emp_no = employees.emp_no
LEFT JOIN titles
ON employees.emp_no = titles.emp_no
WHERE (birth_date BETWEEN '1965-01-01' AND '1965-12-31');