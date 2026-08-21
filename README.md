# Employee Payroll Management System

MySQL database project for employee management, attendance, salary and payroll processing.

A complete **MySQL database project** designed to manage employee information, departments, designations, attendance, salary details, payroll processing, salary auditing, and business reporting.

This project demonstrates practical **SQL and database development concepts** including database design, primary keys, foreign keys, self-referencing relationships, constraints, joins, aggregate functions, views, stored procedures, functions, triggers, indexes, subqueries, and business-oriented SQL queries.

---

## 📌 Project Overview

The **Employee Payroll Management System** provides a structured relational database for managing employee and payroll-related information.

The database maintains employee records along with their:

* Department
* Designation
* Manager
* Joining date
* Basic salary
* Attendance
* Salary components
* Payroll records
* Salary changes and audit history

The project also includes reusable database objects such as **Views, Stored Procedures, Functions, Triggers, and Indexes** to make the database more efficient and maintainable.

---

## 🎯 Project Objectives

* Manage employee information efficiently.
* Organize employees by departments and designations.
* Maintain employee-manager relationships.
* Track employee attendance.
* Store salary components and salary history.
* Calculate employee net salary.
* Generate monthly payroll records.
* Maintain salary change audit records.
* Create reusable views for salary reporting.
* Improve query performance using indexes.
* Perform business analysis using SQL queries.

---

## 🛠️ Technologies Used

| Technology          | Purpose                               |
| ------------------- | ------------------------------------- |
| **MySQL**           | Relational database management system |
| **SQL**             | Database development and querying     |
| **MySQL Workbench** | Database design and SQL execution     |
| **Git**             | Version control                       |
| **GitHub**          | Source code and project documentation |

---

## 🗂️ Project Structure

```text
Employee-Payroll-Management-System/
│
├── SQL/
│   ├── 01_Create-Database.sql.txt
│   ├── 02_Create_Tables.sql.txt
│   ├── 03_Insert_Sample_Data.sql.txt
│   ├── 04_Create_Views.sql.txt
│   ├── 05_Create_Stored_Procedures.sql.txt
│   ├── 06_Create_Functions.sql.txt
│   ├── 07_Create_Triggers.sql.txt
│   ├── 08_Create_Indexes.sql.txt
│   └── 09_Queries.sql.txt
│
└── README.md
```

The repository is organized in execution order, making it easy to create and test the complete database step by step.

---

# 🗄️ Database Design

## Database Name

```sql
EmployeePayrollDB
```

The database is created using:

```sql
CREATE DATABASE EmployeePayrollDB;
USE EmployeePayrollDB;
```

---

# 📊 Database Tables

The project contains the following main tables:

### 1. Department

Stores department information.

**Important columns:**

* `department_id`
* `department_name`

Departments include IT, HR, Finance, Sales, and Operations.

---

### 2. Designation

Stores employee job designations.

Examples:

* Software Developer
* Senior Developer
* HR Executive
* Accountant
* Sales Executive
* Manager

---

### 3. Employee

Stores the main employee information.

**Important columns:**

* `employee_id`
* `first_name`
* `last_name`
* `email`
* `phone`
* `department_id`
* `designation_id`
* `manager_id`
* `joining_date`
* `basic_salary`
* `is_active`

The `manager_id` creates a **self-referencing relationship**, allowing one employee to report to another employee.

---

### 4. Attendance

Stores daily attendance information.

**Attendance statuses:**

* Present
* Absent
* Leave

The database prevents duplicate attendance records for the same employee on the same date using:

```sql
UNIQUE(employee_id, attendance_date)
```

---

### 5. Salary

Stores employee salary components.

**Salary components:**

* Basic Salary
* HRA
* DA
* Allowance
* Deduction

---

### 6. Payroll

Stores monthly payroll information.

It maintains:

* Basic salary
* HRA
* DA
* Allowance
* Deduction
* Net salary
* Payroll month

The database prevents duplicate payroll records for the same employee and payroll month.

---

### 7. SalaryAudit

Maintains a history of salary changes.

It records:

* Employee ID
* Old salary
* New salary
* Date/time of change

This table is populated automatically by a trigger when an employee's basic salary is updated.

---

# 🔗 Database Relationships

The database uses relational constraints to maintain data integrity.

```text
Department
    │
    └──────────< Employee >────────── Designation
                    │
                    ├──────────< Attendance
                    │
                    ├──────────< Salary
                    │
                    ├──────────< Payroll
                    │
                    └────────── Employee
                              (Manager Relationship)

Salary
   │
   └──────────> SalaryAudit
                  ↑
               Trigger
```

### Key Relationships

* One Department → Many Employees
* One Designation → Many Employees
* One Employee → Many Attendance records
* One Employee → Many Salary records
* One Employee → Many Payroll records
* Employee → Employee through `manager_id`
* Salary updates → SalaryAudit through Trigger

Foreign keys are used throughout the database to maintain referential integrity.

---

# 👨‍💼 Employee Management

The system supports employee-related operations such as:

* Adding employees
* Assigning departments
* Assigning designations
* Assigning managers
* Maintaining joining dates
* Maintaining employee salary
* Searching employees
* Filtering employees by department

Sample employee data is included in the project for testing and demonstration.

---

# 🕐 Attendance Management

The Attendance table tracks employee attendance by date.

Example statuses:

```text
Present
Absent
Leave
```

The project also includes an **attendance summary query** that calculates:

* Present days
* Absent days
* Leave days

for each employee.

---

# 💰 Salary Management

The salary module maintains:

```text
Basic Salary
+ HRA
+ DA
+ Allowance
- Deduction
----------------
= Net Salary
```

This salary calculation is implemented through a reusable MySQL function as well as SQL queries.

---

# 🧾 Payroll Processing

The Payroll table stores monthly payroll records for employees.

### Net Salary Formula

```text
Net Salary =
Basic Salary
+ HRA
+ DA
+ Allowance
- Deduction
```

The project includes a stored procedure called:

```sql
GeneratePayroll()
```

which generates payroll records for a specified payroll month.

---

# 👁️ SQL Views

The project contains two reusable views.

## 1. employee_salary_view

Provides employee salary information along with:

* Employee name
* Department
* Designation
* Basic salary
* HRA
* DA
* Allowance
* Deduction
* Net salary

## 2. current_employee_salary_view

Returns the latest salary record for each employee using the most recent `effective_from` date.

---

# ⚙️ Stored Procedures

The project contains three stored procedures.

## 1. GetEmployeesByDepartment

Retrieves employees belonging to a specific department.

```sql
CALL GetEmployeesByDepartment(1);
```

---

## 2. SearchEmployees

Searches employees using employee name and/or department.

```sql
CALL SearchEmployees('Lakshman', NULL);
```

or:

```sql
CALL SearchEmployees(NULL, 1);
```

---

## 3. GeneratePayroll

Generates payroll records for a specified month.

```sql
CALL GeneratePayroll('2026-09-01');
```

The stored procedures are implemented in the database to encapsulate commonly used business operations.

---

# 🧮 User-Defined Function

The project includes a MySQL function:

```sql
CalculateNetSalary()
```

It calculates net salary using:

```text
Basic Salary + HRA + DA + Allowance - Deduction
```

Example:

```sql
SELECT CalculateNetSalary(
    45000,
    9000,
    4500,
    3000,
    1500
) AS net_salary;
```

---

# 🔔 Trigger & Salary Audit

The project implements a trigger:

```text
salary_update_audit
```

The trigger executes automatically after a salary record is updated.

If the employee's basic salary changes, the trigger stores:

```text
Employee ID
Old Salary
New Salary
Changed Time
```

in the `SalaryAudit` table.

This provides a simple **salary change audit mechanism**.

---

# ⚡ Indexing & Query Optimization

Indexes are created on frequently searched or joined columns.

### Indexes implemented

```sql
idx_employee_department
```

on:

```text
Employee(department_id)
```

```sql
idx_employee_salary
```

on:

```text
Employee(basic_salary)
```

```sql
idx_attendance_employee_date
```

on:

```text
Attendance(employee_id, attendance_date)
```

These indexes are intended to improve query performance for common filtering and join operations.

---

# 📈 Business Queries & Reports

The project contains multiple practical SQL queries for database analysis and reporting.

### Included queries

* Display all employees
* Display all departments
* Display all payroll records
* Get employees by department
* Search employees by name
* Search employees by department
* Generate monthly payroll
* Calculate net salary
* Calculate net salary for all employees
* Generate payroll report
* Department-wise payroll summary
* Attendance summary
* Employee-manager report
* Test salary audit trigger
* View employee salary information
* View current employee salary
* Check duplicate payroll records

---

# 🧠 SQL Concepts Demonstrated

This project demonstrates several important SQL and database concepts:

### Database Fundamentals

* Database creation
* Table creation
* Data insertion
* Primary keys
* Foreign keys
* Constraints
* `AUTO_INCREMENT`
* `UNIQUE`
* `CHECK`
* `DEFAULT`

### SQL Querying

* `SELECT`
* `WHERE`
* `JOIN`
* `LEFT JOIN`
* `GROUP BY`
* `ORDER BY`
* `HAVING`
* Aggregate functions
* `CASE`
* Subqueries
* String functions
* Date operations

### Advanced MySQL

* Views
* Stored Procedures
* User-defined Functions
* Triggers
* Indexes
* Self-referencing foreign keys
* Salary auditing
* Payroll generation

---

# 🚀 How to Run the Project

## Step 1: Install MySQL

Install **MySQL Server** and optionally **MySQL Workbench**.

---

## Step 2: Clone the Repository

```bash
git clone https://github.com/KURUVALAKSHMANNA/Employee-Payroll-Management-System.git
```

Then move into the project:

```bash
cd Employee-Payroll-Management-System
```

---

## Step 3: Open MySQL Workbench

Open MySQL Workbench and connect to your MySQL server.

---

## Step 4: Execute SQL Scripts in Order

Run the scripts in the following order:

```text
01_Create-Database.sql.txt
        ↓
02_Create_Tables.sql.txt
        ↓
03_Insert_Sample_Data.sql.txt
        ↓
04_Create_Views.sql.txt
        ↓
05_Create_Stored_Procedures.sql.txt
        ↓
06_Create_Functions.sql.txt
        ↓
07_Create_Triggers.sql.txt
        ↓
08_Create_Indexes.sql.txt
        ↓
09_Queries.sql.txt
```

The repository already follows this logical execution order.

> **Note:** The files currently have `.sql.txt` extensions. If you want them to appear as normal SQL files in GitHub and MySQL tools, rename them from `.sql.txt` to `.sql`.

For example:

```text
01_Create-Database.sql
02_Create_Tables.sql
03_Insert_Sample_Data.sql
```

---

# 🔍 Example Queries

### Display all employees

```sql
SELECT * 
FROM Employee;
```

### Search employees by department

```sql
CALL GetEmployeesByDepartment(1);
```

### Calculate net salary

```sql
SELECT CalculateNetSalary(
    45000,
    9000,
    4500,
    3000,
    1500
) AS net_salary;
```

### View salary information

```sql
SELECT *
FROM employee_salary_view;
```

### View current employee salary

```sql
SELECT *
FROM current_employee_salary_view;
```

---

# 📌 Key Features

| Feature                | Implemented |
| ---------------------- | :---------: |
| Employee Management    |      ✅      |
| Department Management  |      ✅      |
| Designation Management |      ✅      |
| Manager Hierarchy      |      ✅      |
| Attendance Management  |      ✅      |
| Salary Management      |      ✅      |
| Payroll Processing     |      ✅      |
| Net Salary Calculation |      ✅      |
| Salary Audit           |      ✅      |
| SQL Views              |      ✅      |
| Stored Procedures      |      ✅      |
| User-Defined Function  |      ✅      |
| Trigger                |      ✅      |
| Indexes                |      ✅      |
| Business Reports       |      ✅      |
| Duplicate Detection    |      ✅      |

---

# 🎓 Learning Outcomes

Through this project, I gained practical experience in:

* Designing relational databases
* Creating normalized database structures
* Implementing primary and foreign key relationships
* Writing complex SQL queries
* Working with joins and subqueries
* Using aggregate functions for reporting
* Creating reusable database views
* Developing stored procedures
* Creating MySQL functions
* Implementing triggers for auditing
* Applying indexes for query optimization
* Designing payroll calculation logic
* Generating business-oriented reports

---

# 🔮 Future Enhancements

The project can be extended with:

* Employee leave management
* Monthly attendance-based salary calculation
* Tax calculation
* PF/ESI calculation
* Payslip generation
* Role-based database access
* More payroll validation procedures
* Additional audit tables
* Advanced payroll reports
* Performance optimization using `EXPLAIN`
* Database backup and recovery scripts
* Dashboard integration with Power BI or another reporting tool

---

# 👨‍💻 Author

**Lakshmanna K**

B.Tech – Computer Science and Engineering

Email: kuruvalakshmanna4154@gmail.com
LinkedIn: https://www.linkedin.com/in/lakshmanna-kuruva-749250334/
GitHub: https://github.com/KURUVALAKSHMANNA

---

# ⭐ Project Highlights

This project focuses on building a practical **Employee Payroll Database System using MySQL** rather than a simple collection of SQL queries.

It demonstrates how a real-world relational database can combine:

**Database Design → Employee Management → Attendance → Salary Management → Payroll Processing → Reporting → Auditing → Query Optimization**

into one structured database solution.
