# Numeric data types

### Understanding Numeric Data Types in Databases

To ensure that every column in a database table accepts the correct type of data, data types are defined for each column. Data types determine the kind of data that can be stored in each field, ensuring that values are consistent and as expected.

#### Key Concepts of Data Types

- **Purpose**: Data types tell the database management system (DBMS) how to interpret the values stored in each column, maintaining data integrity and format.
- **Common Data Types**: The most frequently used data types include numeric, string, and date and time.

#### Numeric Data Types

Numeric data types are used to store numbers in database columns. They can be broadly categorized into two main types: integer and decimal.

1. **Integer Data Type**:
   - **Definition**: Used for whole numbers without fractional components.
   - **Examples**: `1`, `42`, `1000`
   - **Usage**: Ideal for columns that store counts or quantities, such as product quantity.
   - **Characteristics**: 
     - In MySQL, `TINYINT` can store values from -128 to 127 or 0 to 255 if unsigned.
     - `INT` can store much larger values, ranging from -2,147,483,648 to 2,147,483,647 or 0 to 4,294,967,295 if unsigned.

2. **Decimal Data Type**:
   - **Definition**: Used for numbers that include fractional components.
   - **Examples**: `80.90`, `123.45`
   - **Usage**: Suitable for columns that store prices, measurements, or any values requiring precision, such as total price.
   - **Characteristics**: 
     - Supports fixed-point numbers with specified precision and scale, such as `DECIMAL(10, 2)` which allows up to 10 digits with 2 decimal places.

#### Practical Example: Online Store Database

Consider a table in a database of an online store, which includes columns like `customer_name`, `order_date`, `product_quantity`, and `total_price`.

- **Customer Name**: Uses a string data type to store names.
- **Order Date**: Uses a date data type to store dates.
- **Product Quantity**: Uses an integer data type because it stores whole numbers (e.g., quantity of products).
- **Total Price**: Uses a decimal data type to store prices with fractions (e.g., $80.90).

#### Integer vs. Decimal Data Types

- **Integer**:
  - Stores whole numbers only.
  - Rounds fractional numbers to the nearest whole number.
  - Examples: `TINYINT`, `SMALLINT`, `MEDIUMINT`, `INT`, `BIGINT`.

- **Decimal**:
  - Stores numbers with fractions.
  - Preserves fractional components, ensuring precision.
  - Examples: `DECIMAL`, `NUMERIC`.

#### Summary

By defining appropriate data types for each column, you ensure data consistency and correctness in your database. Numeric data types, particularly integers and decimals, are fundamental for accurately storing numerical values, from whole numbers like quantities to precise values like prices. Understanding and utilizing these data types effectively is crucial for maintaining the integrity and reliability of your database.

# Exercise: Working with numbers (Lab 1)

# String data types
### Understanding String Data Types in Databases

When creating a table in a database, defining the column names and data types is crucial for ensuring data integrity. String data types are essential when a column needs to store a mix of character types, such as alphabet characters, numeric characters, and special characters.

#### Overview of String Data Types

String data types encompass various specific types that allow a column to store textual data. The most commonly used string data types are `CHAR` and `VARCHAR`.

1. **CHAR (Character)**:
   - **Definition**: Used to store fixed-length character strings.
   - **Syntax**: `CHAR(length)`, where `length` is the number of characters the column can hold.
   - **Example**: `CHAR(50)` allocates 50 characters for each entry in the column, even if the actual data is shorter.
   - **Usage**: Best for columns where the length of the data is consistent. For instance, a fixed-length username column can be defined as `CHAR(50)`.
   - **Storage**: If a username like "Mark123" (7 characters) is stored in a `CHAR(50)` column, it occupies 50 characters of space, with padding for the remaining 43 characters.

2. **VARCHAR (Variable Character)**:
   - **Definition**: Used to store variable-length character strings.
   - **Syntax**: `VARCHAR(length)`, where `length` is the maximum number of characters the column can hold.
   - **Example**: `VARCHAR(50)` allows for any input up to a maximum of 50 characters.
   - **Usage**: Ideal for columns where the length of the data varies. For instance, a student name column can be defined as `VARCHAR(50)`.
   - **Storage**: If the student name "Mark Simpson" (12 characters) is stored in a `VARCHAR(50)` column, it occupies only 12 characters of space.

#### Practical Example: Student Table

Consider a student table from a college database that stores student login information. The table has columns for `student_name`, `username`, `password`, and `email_address`.

- **Student Name**: Defined as `VARCHAR(50)` since names vary in length.
- **Username**: Defined as `CHAR(50)` to ensure a consistent length, useful for aligning data in fixed-width fields.
- **Password**: Defined as `VARCHAR(50)` to allow varying lengths for different passwords.
- **Email Address**: Defined as `VARCHAR(100)` to accommodate varying lengths of email addresses.

#### Other Commonly Used String Data Types

- **TINYTEXT**: Stores up to 255 characters, suitable for short paragraphs or brief descriptions.
- **TEXT**: Stores up to 65,535 characters, ideal for articles or longer descriptions.
- **MEDIUMTEXT**: Stores up to 16.7 million characters, useful for large documents such as book texts.
- **LONGTEXT**: Stores up to 4 gigabytes of text, suitable for extensive data like entire books or large logs.

### Summary

String data types are critical for storing textual data in databases. `CHAR` is used for fixed-length strings, ensuring consistent storage space, while `VARCHAR` is used for variable-length strings, optimizing space usage. Other string data types like `TINYTEXT`, `TEXT`, `MEDIUMTEXT`, and `LONGTEXT` provide additional options for storing varying amounts of text. By selecting the appropriate string data type, you can maintain data integrity and optimize storage in your database.

# Exercise: Working with strings (Lab 2)

# Default Values

### Understanding Database Constraints: Ensuring Data Accuracy and Reliability

To maintain the accuracy and reliability of the data in a database, it is essential to limit the types of data that can be stored in a table. This is achieved through the use of database constraints, which enforce rules on the data being inserted or updated. In this overview, you'll learn about the purpose of constraints, specifically the `NOT NULL` and `DEFAULT` constraints, and how to apply them using SQL.

#### Purpose of Database Constraints

Database constraints are rules applied to columns or entire tables to ensure data integrity by restricting the type of data that can be entered. They help to prevent invalid data from being stored and ensure that data operations adhere to the specified rules. If an operation violates a constraint, the database rejects the operation.

#### Types of Constraints

1. **NOT NULL Constraint**:
   - **Purpose**: Ensures that a column cannot have a null value, meaning every entry in this column must contain data.
   - **Example**: In a table recording customer information, the `customer_id` and `customer_name` columns must always contain data. If an attempt is made to insert a null value into either column, the database operation will fail.

   **SQL Implementation**:
   ```sql
   CREATE TABLE Customer (
       customer_id INT NOT NULL,
       customer_name VARCHAR(50) NOT NULL
   );
   ```

   In this example, both `customer_id` and `customer_name` columns are defined with the `NOT NULL` constraint, ensuring they cannot contain null values.

2. **DEFAULT Constraint**:
   - **Purpose**: Sets a default value for a column if no value is specified during data insertion. This is useful for columns that frequently contain a specific value, reducing the need to repeatedly enter the same data.
   - **Example**: In a football club's database, most players might be from Barcelona. By setting a default value of "Barcelona" for the `city` column, the database automatically inserts this value when no other value is provided.

   **SQL Implementation**:
   ```sql
   CREATE TABLE Player (
       player_name VARCHAR(50) NOT NULL,
       city VARCHAR(50) DEFAULT 'Barcelona'
   );
   ```

   Here, the `city` column is given a default value of "Barcelona". When a new player's record is inserted without specifying a city, "Barcelona" will be automatically filled in.

#### Column-Level and Table-Level Constraints

Constraints can be applied at two levels:
- **Column-Level**: The constraint is applied directly to a specific column. For example, `NOT NULL` and `DEFAULT` constraints are typically applied at the column level.
- **Table-Level**: The constraint is applied to the entire table, affecting multiple columns. An example of a table-level constraint is a foreign key, which maintains referential integrity between tables.

**Foreign Key Example**:
A foreign key constraint can be used to link the `customer_id` column in an `Orders` table to the `customer_id` column in a `Customer` table, ensuring that every order references a valid customer.

```sql
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES Customer(customer_id)
);
```

In this example, the `FOREIGN KEY` constraint ensures that the `customer_id` in the `Orders` table corresponds to an existing `customer_id` in the `Customer` table.

### Conclusion

Database constraints are essential tools for maintaining data integrity and reliability. By enforcing rules such as `NOT NULL` and `DEFAULT`, constraints help ensure that only valid and consistent data is entered into a database. Understanding and properly implementing these constraints allows for better data management and prevents errors that could compromise the integrity of the database.

# Working with default values (Lab 3)

# Additional Resources 
Here are some additional resources that explore the meaning and role of different data types in databases, as well as provide examples for how to declare and use data types in SQL:

- [W3schools](https://www.w3schools.com/sql/sql_datatypes.asp)
- [W3resource](https://www.w3resource.com/sql/datatypes.php)
- [LearnSQL](https://learnsql.com/blog/sql-data-types/)
- [Microsoft](https://docs.microsoft.com/en-us/sql/t-sql/data-types/data-types-transact-sql?view=sql-server-ver15)
- [MySQL](https://dev.mysql.com/doc/refman/8.0/en/data-types.html)

These resources should help you gain a better understanding of data types in databases and how to work with them in SQL.
