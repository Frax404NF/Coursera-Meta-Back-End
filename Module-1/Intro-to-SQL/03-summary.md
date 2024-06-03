# What is Structured Query Language?

- **Databases** are used to store and manage data.
- To work with data in databases, you need to interact with them using a language called **Structured Query Language (SQL)**.
- SQL is the **standard language** used with all databases, especially **relational databases**.
- Relational databases, like **MySQL, PostgreSQL, Oracle, and Microsoft SQL Server**, store structured data and require a language like SQL to interact with them.
- SQL instructions are interpreted and executed by a **Database Management System (DBMS)**, which transforms them into a format understood by the underlying database.
- As a web developer, you'll use a DBMS to execute SQL instructions on a database.

# SQL usage

Imagine that you've just been hired to create a database for a college. **First, you need to create tables** to hold data in all aspects of the college. **Then you need to insert data** into these tables and then **modify this data whenever something changes**. That's a lot of work. But it's all possible with the use of **SQL and CRUD operations**.

**CRUD operations** are the most common tasks when working with a database. **CRUD stands for Create, Read, Update, and Delete**. These operations allow you to:

1. **Create**: Add or insert data.
2. **Read**: Retrieve data.
3. **Update**: Modify existing data.
4. **Delete**: Remove data.

Alongside these operations, there are many other things that SQL can do. **Depending on what SQL is used for, it can be divided into several subsections or sub-languages**. These include:

1. **DDL (Data Definition Language)**
2. **DML (Data Manipulation Language)**
3. **DQL (Data Query Language)**
4. **DCL (Data Control Language)**

### Data Definition Language (DDL)

**DDL helps you define data in your database**. Before you can store data, you need to **create the database and related objects like tables**. The **DDL part of SQL** has several commands:

- **CREATE**: Used to create the database and tables.
- **ALTER**: Used to modify the structure of an existing table, such as adding a new column.
- **DROP**: Used to remove objects like tables from the database.

### Data Manipulation Language (DML)

**DML commands are used to manipulate data** within the database, which includes most CRUD operations:

- **INSERT**: Add data to a table by specifying the fields and values to be inserted.
- **UPDATE**: Modify existing data in a table.
- **DELETE**: Remove data from a table.

### Data Query Language (DQL)

**DQL is used to read or retrieve data** stored in the database. It primarily includes:

- **SELECT**: Retrieve data from one or multiple tables, allowing you to specify the fields and apply filters to get the preferred data.

### Data Control Language (DCL)

**DCL is used to control access to the database**. It includes commands to manage user permissions:

- **GRANT**: Give users access privileges to the data.
- **REVOKE**: Remove access privileges from users.

By using these **SQL commands and understanding these sub-languages**, you can efficiently create, manage, and manipulate a database to meet the needs of the college.

# Advantages of SQL

**SQL is the interface or bridge between a relational database and its users** and offers web developers a wide range of advantages. Let's look at a few of them:

1. **Ease of Use**: 
   - **Very little coding skills required**: SQL is just a set of keywords, making it easy to learn and use.
   - **Minimal lines of code**: Basic CRUD operations (Create, Read, Update, Delete) can be performed with few lines of code.
   - **Developer/User-friendly**: It is designed to be easy for both developers and non-developers to use.

2. **Interactivity**:
   - **Write complex queries quickly**: SQL allows developers to write complex queries efficiently, saving time and effort.

3. **Standard Language**:
   - **Universal usage**: SQL can be used with all relational databases like MySQL, PostgreSQL, Oracle, etc.
   - **Abundant support and information**: Because it's standard, there's a lot of community support, documentation, and learning resources available.

4. **Portability**:
   - **Runs on any computer**: SQL code can run on any machine with database software installed.
   - **Cross-platform functionality**: Once written, SQL code can be used on any hardware, operating system, or platform.
   - **Consistency across environments**: SQL code will run the same whether it's on a development machine or a production server.

5. **Comprehensive Language**:
   - **Covers all aspects of database management**: SQL includes commands for creating databases, inserting, updating, and deleting data, retrieving data, sharing data among users, and managing database security.
   - **Sub-languages**: This is achieved through various subsets of SQL:
     - **DDL (Data Definition Language)**: For defining database structures.
     - **DML (Data Manipulation Language)**: For manipulating data.
     - **DQL (Data Query Language)**: For querying data.
     - **DCL (Data Control Language)**: For controlling access to the database.

6. **Efficiency**:
   - **Process large amounts of data quickly**: SQL is optimized for handling and processing large datasets efficiently.

### Summary

**SQL is a simple, standard, portable, comprehensive, and efficient language** that can be used to communicate and work with relational databases. With SQL, you can easily perform a wide range of database tasks, from simple CRUD operations to complex queries and data management. This makes it an invaluable tool for developers and database administrators alike. **You're well on your way to mastering SQL. Well done!**

# SQL syntax introduction

As you might already know, you can interact with the database using **SQL**. But just like with other coding languages, you need experience with **SQL syntax** and its **subsets** before you can make full use of it. Over the next few minutes, you'll learn how to:

1. **Create a database** using the **Data Definition Language (DDL)** subset of SQL.
2. **Populate and modify data** in a database using the **Data Manipulation Language (DML)** subset.
3. **Read and query data** within databases using the **Data Query Language (DQL)** subset of SQL.

To demonstrate SQL syntax and its subsets, I'll show you the SQL commands to develop a database for a college. This demonstration will briefly show each step in the process. You'll explore the language and its subsets in much more detail later.

### Creating the Database

To create the database, I use the **CREATE DATABASE** statement from the **DDL** subset. The syntax is:

```sql
CREATE DATABASE database_name;
```

For example, to create a college database, use:

```sql
CREATE DATABASE college;
```

### Creating Tables

Next, create the tables using the **CREATE TABLE** syntax followed by the table name. For example, to create a student table in the college database:

```sql
CREATE TABLE student (
    ID INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    date_of_birth DATE
);
```

### Populating the Table with Data

To add data to the table, use the **INSERT INTO** syntax from the **DML** subset. The syntax is:

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

For example, to add data to the student table:

```sql
INSERT INTO student (ID, first_name, last_name, date_of_birth)
VALUES (1, 'John', 'Murphy', '2000-01-01');
INSERT INTO student (ID, first_name, last_name, date_of_birth)
VALUES (2, 'Jane', 'Doe', '1999-05-15');
```

### Updating Data

To update or modify data, use the **UPDATE** syntax from the **DML** subset. The syntax is:

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

For example, to update the date of birth for the student with ID 2:

```sql
UPDATE student
SET date_of_birth = '1999-06-15'
WHERE ID = 2;
```

### Deleting Data

To delete data from a table, use the **DELETE** syntax from the **DML** subset. The syntax is:

```sql
DELETE FROM table_name
WHERE condition;
```

For example, to delete the record for the student with ID 3:

```sql
DELETE FROM student
WHERE ID = 3;
```

### Reading Data

To read or query data, use the **SELECT** statement from the **DQL** subset. The syntax is:

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

For example, to find the name of the student with ID 1:

```sql
SELECT first_name, last_name
FROM student
WHERE ID = 1;
```

This will return the name **John Murphy**.

### Summary

You're now familiar with the basics of **SQL syntax** and its **subsets**:

- **DDL (Data Definition Language)**: For defining database structures.
- **DML (Data Manipulation Language)**: For manipulating data.
- **DQL (Data Query Language)**: For querying data.

# Common SQL Commands

SQL (Structured Query Language) is the most widely used database query language designed for retrieving and managing data in a relational database. SQL can perform various operations in the database such as accessing data, describing data, manipulating data, and setting user roles and privileges (permissions).

### Main SQL Command Categories

SQL commands are grouped into four categories depending on their functionality:

1. **Data Definition Language (DDL)**
2. **Data Query Language (DQL)**
3. **Data Manipulation Language (DML)**
4. **Data Control Language (DCL)**
5. **Transaction Control Language (TCL)**

### Data Definition Language (DDL)

**DDL commands** define, delete, and modify tables in a database.

- **CREATE Command**
  - **Purpose**: To create databases or tables.
  - **Syntax**: 
    ```sql
    CREATE TABLE table_name (
      column_name1 datatype(size), 
      column_name2 datatype(size), 
      column_name3 datatype(size)
    );
    ```

- **DROP Command**
  - **Purpose**: To delete a database or a table.
  - **Syntax**: 
    ```sql
    DROP TABLE table_name;
    ```

- **ALTER Command**
  - **Purpose**: To change the structure of tables.
  - **Syntax to add a column**: 
    ```sql
    ALTER TABLE table_name ADD (column_name datatype(size));
    ```
  - **Syntax to add a primary key**: 
    ```sql
    ALTER TABLE table_name ADD PRIMARY KEY (column_name);
    ```

- **TRUNCATE Command**
  - **Purpose**: To remove all records from a table without deleting the table.
  - **Syntax**: 
    ```sql
    TRUNCATE TABLE table_name;
    ```

- **COMMENT Command**
  - **Purpose**: To add comments to SQL statements for documentation.
  - **Syntax**: 
    ```sql
    -- Retrieve all data from a table
    SELECT * FROM table_name;
    ```

### Data Query Language (DQL)

**DQL commands** are used to query and retrieve data from the database.

- **SELECT Command**
  - **Purpose**: To retrieve data from tables.
  - **Syntax**: 
    ```sql
    SELECT * FROM table_name;
    ```

### Data Manipulation Language (DML)

**DML commands** provide the ability to query, delete, and update data in the database.

- **INSERT Command**
  - **Purpose**: To add records to an existing table.
  - **Syntax**: 
    ```sql
    INSERT INTO table_name (column1, column2, column3) VALUES (value1, value2, value3);
    ```

- **UPDATE Command**
  - **Purpose**: To modify or update data in a table.
  - **Syntax**: 
    ```sql
    UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
    ```

- **DELETE Command**
  - **Purpose**: To delete data from a table.
  - **Syntax**: 
    ```sql
    DELETE FROM table_name WHERE condition;
    ```

### Data Control Language (DCL)

**DCL commands** deal with the rights and permissions of users of a database system.

- **GRANT Command**
  - **Purpose**: To provide users with the privileges to access and manipulate the database.
  - **Syntax**: 
    ```sql
    GRANT privilege_type ON object TO user;
    ```

- **REVOKE Command**
  - **Purpose**: To remove permissions from any user.
  - **Syntax**: 
    ```sql
    REVOKE privilege_type ON object FROM user;
    ```

### Transaction Control Language (TCL)

**TCL commands** manage transactions in the database. They allow for grouping SQL statements into logical transactions.

- **COMMIT Command**
  - **Purpose**: To save all changes made during the current transaction.
  - **Syntax**: 
    ```sql
    COMMIT;
    ```

- **ROLLBACK Command**
  - **Purpose**: To undo changes made during the current transaction.
  - **Syntax**: 
    ```sql
    ROLLBACK;
    ```

### Summary

By understanding and using these commands, you can effectively manage and manipulate data within a relational database. Each command serves a specific purpose and fits into one of the categories based on its functionality.

# Additional Resources

Additional resources
The following resources are some additional reading material that introduces you to SQL. These will enable you to enhance your knowledge on SQL syntaxes and common SQL commands (DQL, DML and DDL) that you've learned throughout this lesson.

- Javapoint
(https://www.javatpoint.com/dbms-sql-introduction)

- Beginnersbook
(https://beginnersbook.com/2018/11/introduction-to-sql/)