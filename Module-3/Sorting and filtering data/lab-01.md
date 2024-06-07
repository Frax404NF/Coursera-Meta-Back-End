# Lab ORDER BY and WHERE

Absolutely, I can help you structure the text and SQL code for the exercise:

**Goal**

* Learn how to use SQL's `ORDER BY` and `WHERE` clauses to sort and filter data.

**Objectives**

1. Utilize the `ORDER BY` clause to sort query results.
2. Employ the `WHERE` clause to filter records based on conditions.

**The Chinook Database Example**

This exercise utilizes the popular "Chinook sample" database, commonly used for demonstrations and testing. It's also available on Coursera, where you'll complete this exercise.

The Chinook sample database has the following schema (table structure):

| Column Name          | Data Type | Description                                 |
|-----------------------|-----------|----------------------------------------------|
| CustomerID            | INT       | Unique identifier for a customer             |
| FirstName             | VARCHAR   | Customer's first name                        |
| LastName              | VARCHAR   | Customer's last name                         |
| Company               | VARCHAR   | Customer's company name (if applicable)      |
| Address               | VARCHAR   | Customer's address                          |
| City                  | VARCHAR   | Customer's city                             |
| State                 | VARCHAR   | Customer's state (or province)               |
| Country               | VARCHAR   | Customer's country                           |
| PostalCode            | VARCHAR   | Customer's postal code                       |
| Phone                 | VARCHAR   | Customer's phone number                      |
| Fax                   | VARCHAR   | Customer's fax number (if applicable)        |
| Email                  | VARCHAR   | Customer's email address                     |
| SupportRepId           | INT       | Foreign key referencing the SupportRep table |

**Before You Start**

To proceed with this exercise, ensure you have completed the following steps to set up the Chinook database and create the Customer table within MySQL on Coursera:

1. **Create the Chinook Database and Use It:**

```sql
CREATE DATABASE Chinook;

USE Chinook;
```

2. **Create the Customer Table:**

```sql
CREATE TABLE Customer (
  CustomerId INT NOT NULL,
  FirstName VARCHAR(40) NOT NULL,
  LastName VARCHAR(20) NOT NULL,
  Company VARCHAR(80),
  Address VARCHAR(70),
  City VARCHAR(40),
  State VARCHAR(40),
  Country VARCHAR(40),
  PostalCode VARCHAR(10),
  Phone VARCHAR(24),
  Fax VARCHAR(24),
  Email VARCHAR(60) NOT NULL,
  SupportRepId INT,
  CONSTRAINT PK_Customer PRIMARY KEY (CustomerId)
);
```

3. **Insert Sample Customer Data:**

Insert the provided customer data into the Customer table using the following `INSERT` statements:

**(Copy and paste each statement individually into the terminal)**

```sql
-- Customer 1
INSERT INTO Customer (CustomerId, FirstName, LastName, Company, Address, City, State, Country, PostalCode, Phone, Fax, Email, SupportRepId)
VALUES (1, 'Luís', 'Gonçalves', 'Embraer - Empresa Brasileira de Aeronáutica S.A.', 'Av. Brigadeiro Faria Lima, 2170', 'São José dos Campos', 'SP', 'Brazil', '12227-000', '+55 (12) 3923-5555', '+55 (12) 3923-5566', 'luisg@embraer.com.br', 3);

-- Customer 2 (and so on...)
```

**Instructions**

Try these tasks before continuing to compare your answers with the provided solution:

**Task 1: Display All Customer Data**

Before sorting and filtering, view the existing customer data using a `SELECT` statement that retrieves all data from the Customer table:

```sql
SELECT CustomerID, FirstName, LastName, City, State, Country
FROM Customer;
```

**Task 2: Sort Customer Data by First Name**

Enhance data usability by sorting results alphabetically (A-Z) by first name. Achieve this by adding the `ORDER BY` clause to the previous statement:

```sql
SELECT CustomerID, FirstName, LastName, City, State, Country
FROM Customer
ORDER BY FirstName;
```

**Task 3: Filter Customers by Country**

Simplify customer searches by filtering data based on a specific country. Here, you'll filter for customers from France:

```sql
SELECT *
FROM Customer
WHERE Country = "France";
```

**Additional Exercise (Optional)**

Write a SQL statement to display only the names and countries for customers from Canada, with alphabetical ordering by first name:



