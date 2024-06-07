# SQL Arithmetic Operators

Sure! Here's a more structured and engaging summary of the topic, broken down into key parts:

### Introduction to SQL Arithmetic Operators
Imagine you manage a **corporate database** with **information about hundreds of employees**. How can you efficiently and accurately calculate **salary increases** or adjust **allowances** for all employees? The answer lies in using **SQL arithmetic operators**.

### What Are Operators in SQL?
**Operators** in SQL are like **tools** that help you perform various tasks within a database. Think of them as the **conjunctions** in a sentence or the **operation keys** on a calculator. They enable you to **query and manipulate data** for different purposes.

### Importance of SQL Operators
Why should you care about these operators? When handling data, you'll need to **manipulate and query it**. **SQL operators** are essential for these tasks, allowing you to perform calculations and comparisons. For instance, you can determine how many **leave days** an employee has left or if they are meeting company targets.

### Types of Arithmetic Operators
Let's dive into some common **arithmetic operators** in SQL:

- **Addition (+)**: Adds two values.
- **Subtraction (-)**: Subtracts one value from another.
- **Multiplication (*)**: Multiplies two values.
- **Division (/)**: Divides one value by another.
- **Modulus (%)**: Returns the remainder of a division.

### How Do SQL Arithmetic Operations Work?
When you perform a calculation, an operator takes two **operands** and returns a **result**. For example, the **addition operator** can take the numbers 5 and 5, and return 10. In SQL, you use the **SELECT command** to perform these operations.

### Example Calculations in SQL
Let's illustrate these concepts with some examples:

1. **Addition**:
   ```sql
   SELECT 10 + 15;
   ```
   This returns **25**.

2. **Subtraction**:
   ```sql
   SELECT 15 - 15;
   ```
   This returns **0**.

3. **Multiplication**:
   ```sql
   SELECT 5 * 5;
   ```
   This returns **25**.

4. **Division**:
   ```sql
   SELECT 5 / 5;
   ```
   This returns **1**.

5. **Modulus**:
   ```sql
   SELECT 100 % 10;
   ```
   This returns **0** (since 100 divided by 10 leaves no remainder).

### Performing Basic Operations
To use these operators in SQL, simply combine them with the **SELECT command**:
```sql
SELECT 10 + 15;   -- Addition
SELECT 20 - 5;    -- Subtraction
SELECT 4 * 5;     -- Multiplication
SELECT 20 / 4;    -- Division
SELECT 20 % 3;    -- Modulus
```

### Conclusion
You've now learned about **SQL arithmetic operators** and how to use them to perform basic mathematical operations. With this knowledge, you're ready to apply these operators in more practical and complex ways to manage your corporate database efficiently. Awesome work!

# SQL Arithmetic Operator Examples

In this reading, you'll explore more about the **arithmetic operators** that can be used with **SQL**. You've already learned about the basic arithmetic operators in SQL, such as **addition, subtraction, multiplication, division**, and the **modulus operator**, which gives the remainder of a mathematical division. The main objective here is to present more examples and include some **advanced scenarios**.

#### Arithmetic Operators
**Arithmetic operators** are useful when you want to perform mathematical operations on the data in tables while retrieving them by writing SQL **SELECT queries**. They are used with numerical data stored in database tables.

**Arithmetic operators** can be used in the **SELECT clause** as well as in the **WHERE clause** of a SQL SELECT statement. When used in the WHERE clause, they perform operations on specific rows only, filtering out the data that a particular SQL statement is working on.

The common arithmetic operators are:
- **Addition (+)**
- **Subtraction (-)**
- **Multiplication (*)**
- **Division (/)** 
- **Modulus (%)**

### Using the Addition Operator
The SQL addition operator performs the mathematical addition operation on numerical data within columns in a table. 

**Syntax**:
```sql
SELECT column_name1 + column_name2 FROM table_name;
```

**Example**:
Imagine an **employee** table:

| employee_ID | employee_name | salary | allowance |
|-------------|----------------|--------|-----------|
| 1           | Alex           | 25000  | 1000      |
| 2           | John           | 55000  | 1000      |
| 3           | James          | 52000  | 1000      |
| 4           | Sam            | 30000  | 1000      |

To get the total salaries with allowance added:
```sql
SELECT salary + allowance FROM employee;
```
**Output**:

| Salary + allowance |
|--------------------|
| 26000              |
| 56000              |
| 53000              |
| 31000              |

### Using the Addition Operator in WHERE Clause
Retrieve salaries of employees whose total salary is 25000:

**Syntax**:
```sql
SELECT * 
FROM employee 
WHERE salary + allowance = 25000;
```
**Output**:

| employee_ID | employee_name | salary | allowance |
|-------------|----------------|--------|-----------|
| 1           | Alex           | 24000  | 1000      |
| 4           | Sam            | 24000  | 1000      |

### Using the Subtraction Operator
The SQL subtraction operator performs mathematical subtraction on numerical data within columns in a database table.

**Syntax**:
```sql
SELECT column_name1 - column_name2 FROM table_name;
```

**Example**:
Consider an employee table with a **Tax** column:

| employee_ID | employee_name | salary | allowance | tax |
|-------------|----------------|--------|-----------|-----|
| 1           | Alex           | 24000  | 1000      | 1000|
| 2           | John           | 55000  | 1000      | 2000|
| 3           | James          | 52000  | 1000      | 2000|
| 4           | Sam            | 24000  | 1000      | 1000|

Retrieve salaries after deducting tax:
```sql
SELECT salary - tax FROM employee;
```
**Output**:

| salary – tax |
|--------------|
| 23000        |
| 53000        |
| 50000        |
| 23000        |

### Using the Subtraction Operator in WHERE Clause
Find employees with a salary of 50000 after tax deduction:

**Syntax**:
```sql
SELECT * 
FROM employee 
WHERE salary - tax = 50000;
```
**Output**:

| employee_ID | employee_name | salary | allowance | tax |
|-------------|----------------|--------|-----------|-----|
| 3           | James          | 52000  | 1000      | 2000|

### Using the Multiplication Operator
The SQL multiplication operator multiplies numerical data in columns.

**Syntax**:
```sql
SELECT column_name1 * column_name2 FROM table_name;
```

**Example**:
Double the tax amounts for each employee:
```sql
SELECT tax * 2 FROM employee;
```
**Output**:

| tax * 2 |
|---------|
| 2000    |
| 4000    |
| 4000    |
| 2000    |

### Using the Multiplication Operator in WHERE Clause
Find who pays 4000 in tax after doubling the current tax value:

**Syntax**:
```sql
SELECT *  
FROM employee 
WHERE tax * 2 = 4000;
```
**Output**:

| employee_ID | employee_name | salary | allowance | tax |
|-------------|----------------|--------|-----------|-----|
| 2           | John           | 55000  | 1000      | 2000|
| 3           | James          | 52000  | 1000      | 2000|

### Using the Division Operator
The division operator divides numerical values from one column by those from another column.

**Syntax**:
```sql
SELECT column_name1 / column_name2 FROM table_name;
```

**Example**:
Find the allowance percentage each employee receives:
```sql
SELECT allowance / salary * 100 FROM employee;
```
**Output**:

| allowance / salary * 100 |
|--------------------------|
| 4.1667                   |
| 5.4545                   |
| 5.7692                   |
| 4.1667                   |

### Using the Division Operator in WHERE Clause
Find employees who get an allowance of at least 5%:

**Syntax**:
```sql
SELECT *  
FROM employee 
WHERE allowance / salary * 100 >= 5;
```
**Output**:

| employee_ID | employee_name | salary | allowance | tax |
|-------------|----------------|--------|-----------|-----|
| 2           | John           | 55000  | 3000      | 2000|
| 3           | James          | 52000  | 3000      | 2000|

### Using the Modulus Operator
The modulus operator (%) returns the remainder of a division operation.

**Syntax**:
```sql
SELECT column_name1 % column_name2 FROM table_name;
```

**Example**:
Find if the number of hours worked by each employee is even:
```sql
SELECT hours % 2 FROM employee;
```
**Output**:

| hours % 2 |
|-----------|
| 0         |
| 1         |
| 1         |
| 1         |

### Using the Modulus Operator in WHERE Clause
Filter employees who worked an even number of hours:

**Syntax**:
```sql
SELECT * 
FROM employee 
WHERE hours % 2 = 0;
```
**Output**:

| employee_ID | employee_name | salary | hours | allowance | tax |
|-------------|----------------|--------|-------|-----------|-----|
| 1           | Alex           | 24000  | 10    | 1000      | 1000|

### Conclusion
In this reading, you've learned about the **use of arithmetic operators in SQL**, including addition, subtraction, multiplication, division, and modulus. These examples should help you understand how these operators can be used in both the **SELECT** and **WHERE clauses**.

# Operators in use

### Using SQL Arithmetic Operators in Practice

In this section, we'll take a step further in understanding how to use SQL arithmetic operators with practical data from a corporate table of employee information. This table includes details such as **ID, name**, and **salary**. Let's explore various scenarios where these arithmetic operators can be applied.

#### Adding a Bonus to Each Employee's Salary
Imagine an employer wants to give each employee a $500 bonus and see the updated salaries.

**SQL Query**:
```sql
SELECT salary + 500 FROM employee;
```
- The **SELECT** command retrieves each employee's salary from the **employee** table.
- The **+** operator adds $500 to each salary.

**Result**:
Each employee's salary is increased by $500.

#### Deducting an Amount from Each Employee's Salary
Now, suppose the employer wants to deduct $500 from each employee's salary.

**SQL Query**:
```sql
SELECT salary - 500 FROM employee;
```
- The **SELECT** command retrieves each employee's salary.
- The **-** operator subtracts $500 from each salary.

**Result**:
Each employee's salary is reduced by $500.

#### Doubling Each Employee's Salary
The employer decides to double each employee's current annual salary.

**SQL Query**:
```sql
SELECT salary * 2 FROM employee;
```
- The **SELECT** command retrieves each employee's salary.
- The **\*** operator multiplies each salary by 2.

**Result**:
Each employee's salary is doubled.

#### Calculating Monthly Salary
To determine the monthly salary for each employee, divide the annual salary by 12.

**SQL Query**:
```sql
SELECT salary / 12 FROM employee;
```
- The **SELECT** command retrieves each employee's annual salary.
- The **/** operator divides the annual salary by 12 to calculate the monthly salary.

**Result**:
The monthly salary for each employee is displayed.

#### Checking If Employee IDs Are Even or Odd
To determine if each employee's ID is an even or odd number, use the modulus operator.

**SQL Query**:
```sql
SELECT ID % 2 FROM employee;
```
- The **SELECT** command retrieves each employee's ID.
- The **%** operator divides each ID by 2 and returns the remainder.

**Result**:
- A remainder of 0 indicates an even ID.
- A non-zero remainder indicates an odd ID.

**Output**:
The results show which employee IDs are even and which are odd.

### Conclusion
By following these examples, you have learned how to use SQL arithmetic operators to perform various practical tasks with employee data in a database. These operators are powerful tools for manipulating and analyzing numerical data stored in tables. Keep practicing to enhance your SQL skills further!

# SQL Comparison operators

### Understanding SQL Comparison Operators in Practice

As a database engineer, managing a soccer club's database can involve various tasks such as categorizing players by age or filtering data based on specific criteria. SQL comparison operators are essential tools for these tasks, enabling you to compare values and filter data efficiently. Let's dive into what SQL comparison operators are and how they can be used in a practical setting.

#### What Are SQL Comparison Operators?

**SQL comparison operators** are used to compare two values or expressions, resulting in a boolean outcome (true or false). These operators help to filter data by including or excluding certain records based on specific conditions.

**Common SQL Comparison Operators**:
- **Equal to ( = )**
- **Less than ( < )**
- **Greater than ( > )**
- **Less than or equal to ( <= )**
- **Greater than or equal to ( >= )**
- **Not equal to ( <> or != )**

#### Practical Examples Using an Employee Table

Let's explore these operators using an employee table from a company database, which includes columns such as **ID**, **name**, and **salary**.

##### 1. Finding Employees with a Specific Salary

**Scenario**: The employer wants to identify all employees earning exactly $18,000 per year.

**SQL Query**:
```sql
SELECT * FROM employee WHERE salary = 18000;
```
- **SELECT *:** Retrieves all columns.
- **FROM employee:** Specifies the table.
- **WHERE salary = 18000:** Filters rows where the salary is $18,000.

**Result**: Employees Carl and John earn $18,000 per year.

##### 2. Finding Employees Earning Less Than a Specific Salary

**Scenario**: The employer needs to know which employees earn less than $24,000 per year.

**SQL Query**:
```sql
SELECT * FROM employee WHERE salary < 24000;
```
- **WHERE salary < 24000:** Filters rows where the salary is less than $24,000.

**Result**: Employees Carl and John earn less than $24,000 per year.

##### 3. Finding Employees Earning Less Than or Equal to a Specific Salary

**Scenario**: Determine which employees earn $24,000 or less per year.

**SQL Query**:
```sql
SELECT * FROM employee WHERE salary <= 24000;
```
- **WHERE salary <= 24000:** Filters rows where the salary is less than or equal to $24,000.

**Result**: Four employees earn $24,000 or less per year.

##### 4. Finding Employees Earning More Than or Equal to a Specific Salary

**Scenario**: Identify employees earning $24,000 or more per year.

**SQL Query**:
```sql
SELECT * FROM employee WHERE salary >= 24000;
```
- **WHERE salary >= 24000:** Filters rows where the salary is greater than or equal to $24,000.

**Result**: Three employees earn $24,000 or more per year.

##### 5. Finding Employees Earning a Salary Not Equal to a Specific Amount

**Scenario**: Find employees whose salary is not equal to $24,000 per year.

**SQL Query**:
```sql
SELECT * FROM employee WHERE salary <> 24000;
```
- **WHERE salary <> 24000:** Filters rows where the salary is not $24,000.

**Result**: Three employees have salaries not equal to $24,000 per year.

### Conclusion

SQL comparison operators are crucial for querying and filtering data in a database. By using these operators, you can perform tasks such as categorizing players by age or identifying employees based on salary criteria efficiently. You've now learned how to utilize these operators practically, enhancing your ability to manage and analyze data in a database. Keep practicing to master these essential SQL skills!

# SQL Comparison operator examples

### SQL Comparison Operators: Advanced Examples

Comparison operators in SQL are used to form conditions for filtering data, primarily within the `WHERE` clause of a `SELECT` statement. These operators are essential for querying databases effectively. Let's refresh our understanding of these operators and explore advanced scenarios using them.

#### SQL Comparison Operators

**Operators and Their Functions**:
- **=**: Checks for equality
- **<>** or **!=**: Checks for inequality
- **>**: Checks if a value is greater than another
- **>=**: Checks if a value is greater than or equal to another
- **<**: Checks if a value is less than another
- **<=**: Checks if a value is less than or equal to another

#### Practical Examples

Consider an `employee` table with the following data:

| employee_ID | employee_name | salary | hours | allowance | tax  |
|-------------|---------------|--------|-------|-----------|------|
| 1           | Alex          | 24000  | 10    | 1000      | 1000 |
| 2           | John          | 55000  | 11    | 3000      | 2000 |
| 3           | James         | 52000  | 7     | 3000      | 2000 |
| 4           | Sam           | 24000  | 11    | 1000      | 1000 |

##### Using the Equality Operator (`=`)

**Scenario**: Retrieve data for the employee with `employee_ID` 1.

**SQL Query**:
```sql
SELECT * FROM employee WHERE employee_ID = 1;
```

**Result**:
| employee_ID | employee_name | salary | hours | allowance | tax  |
|-------------|---------------|--------|-------|-----------|------|
| 1           | Alex          | 24000  | 10    | 1000      | 1000 |

**Scenario**: Retrieve data for the employee named James.

**SQL Query**:
```sql
SELECT * FROM employee WHERE employee_name = 'James';
```

**Result**:
| employee_ID | employee_name | salary | hours | allowance | tax  |
|-------------|---------------|--------|-------|-----------|------|
| 3           | James         | 52000  | 7     | 3000      | 2000 |

##### Using the Inequality Operator (`<>` or `!=`)

**Scenario**: Identify employees with salaries not equal to $24,000.

**SQL Query**:
```sql
SELECT * FROM employee WHERE salary <> 24000;
```

**Result**:
| employee_ID | employee_name | salary | hours | allowance | tax  |
|-------------|---------------|--------|-------|-----------|------|
| 2           | John          | 55000  | 11    | 3000      | 2000 |
| 3           | James         | 52000  | 7     | 3000      | 2000 |

You can use `!=` instead of `<>` for the same result.

##### Using the Greater Than Operator (`>`)

**Scenario**: Find employees earning more than $50,000.

**SQL Query**:
```sql
SELECT * FROM employee WHERE salary > 50000;
```

**Result**:
| employee_ID | employee_name | salary | hours | allowance | tax  |
|-------------|---------------|--------|-------|-----------|------|
| 2           | John          | 55000  | 11    | 3000      | 2000 |
| 3           | James         | 52000  | 7     | 3000      | 2000 |

##### Using the Greater Than or Equal Operator (`>=`)

**Scenario**: Identify employees paying a tax amount of $1,000 or more.

**SQL Query**:
```sql
SELECT * FROM employee WHERE tax >= 1000;
```

**Result**:
| employee_ID | employee_name | salary | hours | allowance | tax  |
|-------------|---------------|--------|-------|-----------|------|
| 1           | Alex          | 24000  | 10    | 1000      | 1000 |
| 2           | John          | 55000  | 11    | 3000      | 2000 |
| 3           | James         | 52000  | 7     | 3000      | 2000 |
| 4           | Sam           | 24000  | 11    | 1000      | 1000 |

##### Using the Less Than Operator (`<`)

**Scenario**: Determine which employees receive an allowance below $2,500.

**SQL Query**:
```sql
SELECT * FROM employee WHERE allowance < 2500;
```

**Result**:
| employee_ID | employee_name | salary | hours | allowance | tax  |
|-------------|---------------|--------|-------|-----------|------|
| 1           | Alex          | 24000  | 10    | 1000      | 1000 |
| 4           | Sam           | 24000  | 11    | 1000      | 1000 |

##### Using the Less Than or Equal Operator (`<=`)

**Scenario**: Determine which employees have worked 10 hours or less.

**SQL Query**:
```sql
SELECT * FROM employee WHERE hours <= 10;
```

**Result**:
| employee_ID | employee_name | salary | hours | allowance | tax  |
|-------------|---------------|--------|-------|-----------|------|
| 1           | Alex          | 24000  | 10    | 1000      | 1000 |
| 3           | James         | 52000  | 7     | 3000      | 2000 |

### Conclusion

By using SQL comparison operators, you can filter data based on various conditions, enhancing your ability to retrieve meaningful information from a database. These examples illustrate how to apply these operators in different scenarios to achieve precise data filtering. Keep practicing with different datasets to master these SQL skills.

# Additional Resources 

1. [W3Schools](https://www.w3schools.com/sql/sql_operators.asp)
2. [Javatpoint](https://www.javatpoint.com/sql-operators)
3. [Tutorialspoint](https://www.tutorialspoint.com/sql/sql-operators.htm)
4. [w3resource](https://www.w3resource.com/sql/operators/index.php)

These resources provide extra knowledge on SQL Arithmetic operators and Comparison operators. You can explore them to deepen your understanding of these concepts.