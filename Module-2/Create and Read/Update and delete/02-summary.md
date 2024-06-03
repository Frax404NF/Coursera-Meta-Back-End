# CREATE and DROP database

Creating and managing databases for an online bookstore involves understanding and using SQL (Structured Query Language) commands effectively. In this guide, you will learn how to create and drop databases using SQL syntax, ensuring your database structure is ready to store and manage vast amounts of data efficiently.

### Creating a Database

To create a database using SQL, follow these steps:

1. **Determine the Database Purpose**:
   - Understand the data you need to store. For an online bookstore, this includes book titles, authors, customers, and sales records.

2. **Use SQL Commands**:
   - The basic command to create a database is straightforward.

   **SQL Syntax**:
   ```sql
   CREATE DATABASE bookstore_db2;
   ```

   - Here, `CREATE DATABASE` is the SQL command to create a new database, and `bookstore_db2` is the name of your new database. Ensure the database name is meaningful, relevant, unique, and no more than 63 characters long.

3. **Execution**:
   - Run the SQL command in your database management tool (e.g., MySQL Workbench, phpMyAdmin, or any SQL command-line interface). Upon successful execution, the new database will be created and listed in your database management tool.

### Example:

```sql
CREATE DATABASE bookstore_db2;
```

After running this command, you should see `bookstore_db2` listed in your database management tool.

### Dropping a Database

To remove or delete a database, use the `DROP DATABASE` command. This command should be used with caution, as it will permanently delete the database and all of its data.

**SQL Syntax**:

```sql
DROP DATABASE bookstore_db2;
```

### Example:

```sql
DROP DATABASE bookstore_db2;
```

When you run this command, the `bookstore_db2` database will be deleted. Make sure you really want to remove the database, as this action cannot be undone.

### Summary

By using the `CREATE DATABASE` and `DROP DATABASE` SQL commands, you can easily manage your databases. Here’s a quick recap:

- **Creating a Database**:
  ```sql
  CREATE DATABASE database_name;
  ```

- **Dropping a Database**:
  ```sql
  DROP DATABASE database_name;
  ```

Ensure you have a clear understanding of the purpose of each database and use meaningful, unique names for better management and documentation. Always be cautious when dropping databases to avoid accidental data loss.

# CREATE TABLE statement

Creating tables in a database is an essential step in organizing and storing your data. In SQL, this is done using the `CREATE TABLE` statement. Below, we'll go through the process of creating a table step-by-step, ensuring that you can set up your database tables correctly.

### Syntax for Creating a Table

The basic syntax for creating a table in SQL is as follows:

```sql
CREATE TABLE table_name (
    column1_name data_type1,
    column2_name data_type2,
    ...
);
```

### Steps to Create a Table

1. **Start with the `CREATE TABLE` Command**:
   - This command tells SQL that you want to create a new table.

2. **Name Your Table**:
   - After `CREATE TABLE`, specify the name of your table. For example, `customers`.

3. **Define the Columns and Data Types**:
   - Within parentheses, list the columns you want to include in the table, followed by their data types. Each column should be separated by a comma.

### Example: Creating a Customer Table

Assume you already have a database named `bookstore_db`. We'll create a `customers` table to store customer information like names and phone numbers.

#### SQL Statement

```sql
CREATE TABLE customers (
    customer_name VARCHAR(100),
    phone_number INT
);
```

#### Explanation

- `CREATE TABLE customers`: This creates a table named `customers`.
- `customer_name VARCHAR(100)`: Creates a column named `customer_name` that can hold up to 100 characters of text.
- `phone_number INT`: Creates a column named `phone_number` that holds integer values, suitable for storing phone numbers as whole numbers.

### Running the SQL Statement

To execute this SQL statement, follow these steps:

1. Open your SQL database management tool (e.g., MySQL Workbench, phpMyAdmin).
2. Select your database (e.g., `bookstore_db`).
3. Open a new SQL query window.
4. Enter the `CREATE TABLE` statement.
5. Execute the statement.

After executing the statement, the `customers` table will be created within the `bookstore_db` database, ready to store customer names and phone numbers.

### Important Points

- **Database Requirement**: Ensure that you have a database selected or created before you create tables.
- **Naming Conventions**: Use meaningful and consistent names for your tables and columns to make the database easier to understand and maintain.
- **Data Types**: Choose appropriate data types for each column to ensure data integrity and efficient storage.

### Additional Columns and Constraints

You can add more columns and constraints to ensure data integrity. For example:

```sql
CREATE TABLE customers (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    phone_number VARCHAR(15) NOT NULL
);
```

- `customer_id INT AUTO_INCREMENT PRIMARY KEY`: Adds a unique identifier for each customer and auto-increments the value for each new record.
- `NOT NULL`: Ensures that the `customer_name` and `phone_number` fields cannot be empty.

### Summary

Creating tables using SQL involves defining the table structure, including column names and data types. By following the syntax and steps provided, you can effectively create and organize tables within your database, ensuring your data is stored correctly and is easily accessible.

# ALTER TABLE statement

Altering database tables is a fundamental skill for database developers as it allows for the modification of table structures to accommodate changing data requirements. Let's explore how to add, remove, and modify columns using SQL syntax with practical examples.

### SQL `ALTER TABLE` Statement Syntax

The `ALTER TABLE` statement is used to make changes to an existing table's structure. The basic syntax for altering a table involves the following components:

1. **Adding Columns**:
   ```sql
   ALTER TABLE table_name
   ADD column_name data_type;
   ```

2. **Dropping Columns**:
   ```sql
   ALTER TABLE table_name
   DROP COLUMN column_name;
   ```

3. **Modifying Columns**:
   ```sql
   ALTER TABLE table_name
   MODIFY column_name new_data_type;
   ```

### Example Scenario: Student Table in College Database

Let's assume we have a `students` table in a `college` database with the following structure:
- `student_id INT`
- `name VARCHAR(100)`
- `email VARCHAR(255)`

We will perform the following operations:
1. Add three new columns: `age`, `country`, and `nationality`.
2. Drop the `nationality` column.
3. Modify the `country` column to increase its character limit.

### Adding Columns

To add new columns `age`, `country`, and `nationality`:

```sql
ALTER TABLE students
ADD (
    age INT,
    country VARCHAR(50),
    nationality VARCHAR(255)
);
```

### Dropping a Column

To remove the `nationality` column:

```sql
ALTER TABLE students
DROP COLUMN nationality;
```

### Modifying a Column

To change the `country` column's character limit from 50 to 100:

```sql
ALTER TABLE students
MODIFY country VARCHAR(100);
```

### Putting It All Together

Here’s a step-by-step demonstration of how you would perform these operations:

1. **Add Columns**:
   ```sql
   ALTER TABLE students
   ADD (
       age INT,
       country VARCHAR(50),
       nationality VARCHAR(255)
   );
   ```

2. **Drop a Column**:
   ```sql
   ALTER TABLE students
   DROP COLUMN nationality;
   ```

3. **Modify a Column**:
   ```sql
   ALTER TABLE students
   MODIFY country VARCHAR(100);
   ```

### Executing the Statements

- **Add Columns**:
   ```sql
   ALTER TABLE students
   ADD (
       age INT,
       country VARCHAR(50),
       nationality VARCHAR(255)
   );
   ```

   This statement adds the `age`, `country`, and `nationality` columns to the `students` table.

- **Drop a Column**:
   ```sql
   ALTER TABLE students
   DROP COLUMN nationality;
   ```

   This statement removes the `nationality` column from the `students` table.

- **Modify a Column**:
   ```sql
   ALTER TABLE students
   MODIFY country VARCHAR(100);
   ```

   This statement changes the `country` column to allow up to 100 characters.

### Conclusion

By understanding and using the `ALTER TABLE` statement, you can effectively manage and restructure your database tables to meet evolving data needs. This flexibility is crucial for maintaining robust and adaptable database systems. Well done on mastering these essential SQL operations!

# INSERT statement

Adding new rows to existing tables is a fundamental task in database management. In this guide, we'll learn how to use the SQL `INSERT INTO` statement to add data to tables. By the end of this guide, you should be able to understand the `INSERT INTO` syntax and insert data into tables effectively.

### SQL `INSERT INTO` Syntax

To write an `INSERT INTO` statement:
1. Begin with the `INSERT INTO` clause.
2. Specify the table name.
3. List the column names in parentheses, separated by commas.
4. Use the `VALUES` keyword.
5. List the values for each column in parentheses, in the same order as the columns.

### Basic Syntax
```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

### Example 1: Inserting a Single Row

Let's assume we have a table called `players` with the following columns: `ID`, `Name`, `Age`, and `StartDate`. Here’s how to insert a single row into this table:

```sql
INSERT INTO players (ID, Name, Age, StartDate)
VALUES (1, 'Yuval', 25, '2020-10-15');
```

### Example 2: Inserting Multiple Rows

To insert multiple rows, we can extend the `VALUES` clause to include multiple sets of values, each enclosed in parentheses and separated by commas.

```sql
INSERT INTO players (ID, Name, Age, StartDate)
VALUES
    (2, 'Mark', 27, '2020-10-12'),
    (3, 'Karl', 26, '2020-10-07');
```

### Important Considerations
- **Order of Columns and Values**: Ensure the order of values matches the order of columns.
- **Data Types**: Each value must match the data type of its respective column.
- **String and Date Values**: Enclose non-numeric values (like strings and dates) in single quotes.

### Using `SELECT *` to Verify Data

After inserting data, you can use the `SELECT` statement to verify the entries:

```sql
SELECT * FROM players;
```

This statement retrieves all columns from the `players` table and displays the data.

### Example Scenario: College Database

Imagine we have a `students` table in a `college` database with the columns: `student_id`, `name`, `email`, `age`, `country`. Here's how to add new students:

1. **Inserting Single Row**:
   ```sql
   INSERT INTO students (student_id, name, email, age, country)
   VALUES (1, 'Alice', 'alice@example.com', 21, 'USA');
   ```

2. **Inserting Multiple Rows**:
   ```sql
   INSERT INTO students (student_id, name, email, age, country)
   VALUES
       (2, 'Bob', 'bob@example.com', 22, 'Canada'),
       (3, 'Charlie', 'charlie@example.com', 23, 'UK');
   ```

3. **Verify Data**:
   ```sql
   SELECT * FROM students;
   ```

### Summary

The `INSERT INTO` statement is a powerful tool in SQL for adding data to tables. By understanding its syntax and using it correctly, you can efficiently manage and populate your database tables. Whether you're adding a single row or multiple rows, following the correct syntax and order ensures your data is accurately inserted. Happy coding!

# Creating tables

Creating a table in a database is a fundamental task that allows you to organize and store data efficiently. This guide will help you understand how to use the `CREATE TABLE` statement in SQL to create a table and ensure you follow best practices when doing so.

### Important Points on Creating a Table

1. **Meaningful Names**: Always give meaningful names to your table and columns. This makes your database easier to understand and maintain.
2. **Data Types**: Data types vary across database systems (e.g., `NUMBER` in Oracle, `INT` in MySQL). Always refer to the documentation of the database system you are using.
3. **Length Specification**: Specify appropriate lengths for data types where applicable. This helps optimize storage and performance.
4. **Usage of VARCHAR**: For text-based data, use `VARCHAR` as it saves space by using 1 byte per character plus 2 bytes for the length.

### Using the `CREATE TABLE` Statement

The `CREATE TABLE` statement is part of the Data Definition Language (DDL) subset of SQL, used to create a new table in a database.

### Syntax Overview

Here's a general syntax for the `CREATE TABLE` statement:
```sql
CREATE TABLE table_name (
    column1 datatype(size),
    column2 datatype(size),
    ...
);
```

### Example: Creating a Customers Table

Let's create a table named `customers` with various columns to store customer data. Here is the SQL syntax for this table:

```sql
CREATE TABLE customers (
    CustomerId INT,
    FirstName VARCHAR(40),
    LastName VARCHAR(20),
    Company VARCHAR(80),
    Address VARCHAR(70),
    City VARCHAR(40),
    State VARCHAR(40),
    Country VARCHAR(40),
    PostalCode VARCHAR(10),
    Phone VARCHAR(20),
    Fax VARCHAR(20),
    Email VARCHAR(100),
    SupportRapid INT
);
```

### Explanation

- **Table Name**: `customers`
- **Columns and Data Types**:
  - `CustomerId`: `INT` (Integer value to store customer IDs)
  - `FirstName`: `VARCHAR(40)` (Variable character field to store first names up to 40 characters)
  - `LastName`: `VARCHAR(20)` (Variable character field to store last names up to 20 characters)
  - `Company`: `VARCHAR(80)` (Variable character field to store company names up to 80 characters)
  - `Address`: `VARCHAR(70)` (Variable character field to store addresses up to 70 characters)
  - `City`: `VARCHAR(40)` (Variable character field to store city names up to 40 characters)
  - `State`: `VARCHAR(40)` (Variable character field to store state names up to 40 characters)
  - `Country`: `VARCHAR(40)` (Variable character field to store country names up to 40 characters)
  - `PostalCode`: `VARCHAR(10)` (Variable character field to store postal codes up to 10 characters)
  - `Phone`: `VARCHAR(20)` (Variable character field to store phone numbers up to 20 characters)
  - `Fax`: `VARCHAR(20)` (Variable character field to store fax numbers up to 20 characters)
  - `Email`: `VARCHAR(100)` (Variable character field to store email addresses up to 100 characters)
  - `SupportRapid`: `INT` (Integer value to store a support rapid field)

### Best Practices

- **Naming Conventions**: Use clear and consistent naming conventions for tables and columns.
- **Choosing Data Types**: Choose the most appropriate data type for each column based on the kind of data it will store.
- **Specifying Lengths**: For `VARCHAR` columns, choose a length that fits the expected maximum data size to optimize storage.

### Summary

Creating tables in SQL involves using the `CREATE TABLE` statement along with specifying appropriate data types and lengths for each column. Following best practices in naming and data type selection ensures your tables are efficient and easy to maintain. This guide should help you create well-structured tables for your database needs.

# Exercise: Create Database, create table and insert data (Lab 1)

# SELECT statement

Querying data from a database using the SQL `SELECT` statement is a fundamental task that allows you to retrieve and manipulate data efficiently. In this guide, we'll explore the syntax and usage of the `SELECT` statement for various querying tasks, including basic data retrieval, mathematical calculations, date and time queries, and concatenation functions.

### Basic SQL `SELECT` Statement Syntax

The basic syntax of a `SELECT` statement is:
```sql
SELECT column1, column2, ...
FROM table_name;
```

- **SELECT**: This keyword is used to specify the columns you want to retrieve.
- **FROM**: This keyword specifies the table from which to retrieve the data.
- **column1, column2, ...**: These are the columns you want to select.

### Example: Retrieving Data from a Single Column

If you want to retrieve the names of players from the `players` table, you would use:
```sql
SELECT name
FROM players;
```

### Example: Retrieving Data from Multiple Columns

To retrieve the name and skill level of each player, use:
```sql
SELECT name, skill_level
FROM players;
```

### Example: Retrieving Data from All Columns

There are two methods to retrieve all columns from a table:

1. **List All Columns**:
    ```sql
    SELECT id, name, age, skill_level
    FROM players;
    ```

2. **Use an Asterisk (`*`)**:
    ```sql
    SELECT *
    FROM players;
    ```

### Performing Calculations in SQL

You can perform mathematical operations directly within a `SELECT` statement. For instance, to calculate the age of players next year, you can use:
```sql
SELECT name, age, age + 1 AS age_next_year
FROM players;
```

### Date and Time Queries

SQL allows you to handle date and time data efficiently. For example, to retrieve players who joined after a specific date:
```sql
SELECT name, join_date
FROM players
WHERE join_date > '2023-01-01';
```

### Concatenation Functions

You can concatenate strings in SQL to combine column values. For example, to create a full name from first and last names:
```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM players;
```

### Examples in Action

Let's see these concepts in action using a `players` table:

#### Retrieve Player Names
```sql
SELECT name
FROM players;
```

#### Retrieve Player Names and Skill Levels
```sql
SELECT name, skill_level
FROM players;
```

#### Retrieve All Data from Players Table
```sql
SELECT *
FROM players;
```

#### Calculate Players' Ages Next Year
```sql
SELECT name, age, age + 1 AS age_next_year
FROM players;
```

#### Retrieve Players Who Joined After a Specific Date
```sql
SELECT name, join_date
FROM players
WHERE join_date > '2023-01-01';
```

#### Concatenate First and Last Names
```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM players;
```

### Conclusion

By mastering the `SELECT` statement, you can efficiently query data from your database. Whether you need to retrieve specific columns, perform calculations, handle date and time data, or concatenate strings, the `SELECT` statement provides the flexibility to meet your needs. With these examples and explanations, you should be well-equipped to perform various data retrieval tasks using SQL.

# Exercise: Practicing table creation

# Additional Resources 

- [Tutorialspoint](https://www.tutorialspoint.com/sql/index.htm)
- [Javatpoint](https://www.javatpoint.com/sql-tutorial)
- [Tutorialrepublic](https://www.tutorialrepublic.com/sql-tutorial/)

