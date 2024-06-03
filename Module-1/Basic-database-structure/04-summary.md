# What are tables in databases?
### Understanding Database Tables

A database table is a fundamental structure in a database that organizes data in a logical way. Let's break down the key concepts:

#### 1. **Database Table**

- **Definition**: A table is a collection of data organized in rows and columns.
- **Relation**: In a database with multiple tables, these tables are called relations because they relate to each other.
- **Entity/Object**: Conceptually, a table is also known as an entity in databases. In object-oriented databases, it's referred to as an object. Both terms mean the same thing in this context.

#### 2. **Columns (Fields/Attributes)**

- **Definition**: Columns are the vertical part of a table and hold data attributes. Each column has a unique name and a specified data type.
- **Data Types**: Data types define what type of data a column can hold. Common data types include:
  - **Integer**: Whole numbers.
  - **Character/String**: Textual data.
  - **Date/Time**: Dates and times.
  - **Binary**: Images, files, and other binary data.

#### 3. **Rows (Records)**

- **Definition**: Rows are the horizontal part of a table and represent individual records.
- **Record**: A record is a combination of columns that contains data. Each row in the table corresponds to a single record.

#### 4. **Domains**

- **Definition**: A domain is a set of legal values that a field can hold. It ensures that data in a column adheres to predefined rules.
- **Example**: A numeric domain only allows numbers, while a string domain allows text.

#### 5. **Primary Key**

- **Definition**: A primary key is a column or a combination of columns that uniquely identifies each row in a table.
- **Uniqueness**: Each value in the primary key column(s) must be unique.
- **Example**: In an employee table, the `ID` column is often the primary key because each `ID` is unique.

### Example: Employee Table

Imagine a table called `Employee` that holds data about employees in a company. 

#### Columns:

- **ID**: Integer (Primary Key)
- **FirstName**: String
- **LastName**: String
- **Role**: String
- **DateOfBirth**: Date

#### Rows:

Each row represents an individual employee's record, for example:

- Row 1: (1, "John", "Doe", "Developer", "1985-06-15")
- Row 2: (2, "Jane", "Smith", "Designer", "1990-08-23")

### Summary

- **Tables**: Organize data in rows and columns.
- **Columns**: Also called fields or attributes, each with a unique name and data type.
- **Rows**: Also called records, each representing a single data entry.
- **Domains**: Define the set of legal values for a column.
- **Primary Key**: Ensures each row is uniquely identifiable.

By understanding these fundamental concepts, you can effectively structure and manage data within a relational database.

# Tables overview

### Tables in Relational Databases

Tables are the fundamental building blocks of relational databases. They store and organize data in a structured way, making it easy to retrieve and manipulate. Let’s delve deeper into their structure, data types, keys, and constraints.

#### Structure of a Table

- **Rows and Columns**: 
  - **Rows (Records/Tuples)**: Each row represents a single record in the table.
  - **Columns (Fields/Attributes)**: Columns define the attributes of the data, with each column having a unique name and a specified data type.

- **Cells**: The intersection of a row and a column is a cell, where an individual data item is stored.

#### Data Types

Every column in a table has a specific data type, which defines the kind of data that can be stored in that column. Some common data types include:

- **Numeric Data Types**: `INT`, `TINYINT`, `BIGINT`, `FLOAT`, `REAL`
- **Date and Time Data Types**: `DATE`, `TIME`, `DATETIME`
- **Character and String Data Types**: `CHAR`, `VARCHAR`
- **Binary Data Types**: `BINARY`, `VARBINARY`
- **Miscellaneous Data Types**: `CLOB` (Character Large Object), `BLOB` (Binary Large Object)

#### Example of a Student Table

Consider a table named `Student` with the following columns:

- **StudentID**: `INT`
- **FirstName**: `VARCHAR`
- **LastName**: `VARCHAR`
- **DateOfBirth**: `DATE`
- **HomeAddress**: `VARCHAR`
- **Faculty**: `VARCHAR`

This table would have multiple rows, each representing a student record.

### Primary and Foreign Keys

#### Primary Key

- **Definition**: A primary key is a column or a set of columns that uniquely identifies each row in the table.
- **Example**: In the `Student` table, `StudentID` can serve as the primary key because it uniquely identifies each student.

- **Composite Primary Key**: Sometimes, a single column cannot uniquely identify a row. In such cases, a combination of columns is used as the primary key.
  - **Example**: In an `Employee` table, `EMP_ID` and `DEPT_ID` together can form a composite primary key.

#### Foreign Key

- **Definition**: A foreign key is a column or set of columns in one table that references the primary key in another table, establishing a relationship between the two tables.
- **Example**: If there’s a `Department` table with a `DepartmentID` as its primary key, the `Student` table could have a `DepartmentID` column as a foreign key to establish a relationship between students and departments.

### Integrity Constraints

Integrity constraints ensure the accuracy and consistency of data within the database.

#### Key Constraints

- **Primary Key Constraint**: Ensures that the primary key column(s) cannot have NULL values and must be unique across all rows.
- **Example**: In the `Student` table, the `StudentID` must be unique and not NULL.

#### Domain Constraints

- **Definition**: These constraints ensure that the values in a column fall within a specified range or set of values.
- **Example**: A phone number column should not exceed 10 digits, and a `FirstName` column should only store string data.

#### Referential Integrity Constraints

- **Definition**: Ensure that a foreign key value always matches an existing value in the referenced primary key column.
- **Example**: In the `Student` table, the `DepartmentID` (foreign key) must correspond to a valid `DepartmentID` in the `Department` table.

### Summary

- **Tables**: Store data in rows and columns.
- **Columns**: Each with a specific data type.
- **Rows**: Each represents a single record.
- **Primary Key**: Uniquely identifies each row.
- **Foreign Key**: Establishes relationships between tables.
- **Constraints**: Ensure data integrity and consistency (key, domain, and referential integrity constraints).

By understanding these concepts, you can effectively design and manage relational databases, ensuring data is organized, accurate, and easily accessible.

# Database structure overview

### Basic Database Structure

Understanding the basic structure of a database is crucial for effectively organizing and managing data. This includes knowing about tables, fields (or attributes), records, keys, and the relationships between tables.

#### Key Components of Database Structure
![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/8y07EVOGQMStOxFThvDEjg_092eeba0e42347d1830b1b7298cf22a1_C1M1L4-Item-03---Image1.png?expiry=1717545600000&hmac=sieNWSB6dn1Puy7w1RhJn1w6GC8UJ7eRaCXIxVyFbEo)

1. **Tables (Entities)**:
   - **Definition**: Tables are where data is stored. Each table represents a type of entity and contains multiple records (rows).
   - **Example**: A `Student` table that stores data about students.

2. **Attributes (Fields)**:
   - **Definition**: Attributes are details about the table or entity, describing the properties of the entity.
   - **Example**: In a `Student` table, attributes could include `StudentID`, `FirstName`, `LastName`, `DateOfBirth`, etc.

3. **Fields (Columns)**:
   - **Definition**: Fields are the columns in a table used to capture attributes.
   - **Example**: The `FirstName` field in a `Student` table captures the first name of each student.

4. **Records (Rows/Tuples)**:
   - **Definition**: A record is a single row in a table that contains data for one entity.
   - **Example**: A row in the `Student` table might contain data like `1, John, Doe, 2000-01-01`.

5. **Primary Key**:
   - **Definition**: A primary key is a unique identifier for each record in a table.
   - **Example**: `StudentID` in the `Student` table.

6. **Foreign Key**:
   - **Definition**: A foreign key is a field in one table that links to the primary key of another table, establishing a relationship between the tables.
   - **Example**: `DepartmentID` in the `Student` table referencing `DepartmentID` in the `Department` table.

#### Data Types

Each field (column) in a table has a specific data type, determining what kind of data it can store. Common data types include:

- **Numeric**: `INT`, `TINYINT`, `BIGINT`, `FLOAT`, `REAL`
- **Date and Time**: `DATE`, `TIME`, `DATETIME`
- **Character and String**: `CHAR`, `VARCHAR`
- **Binary**: `BINARY`, `VARBINARY`
- **Miscellaneous**: `CLOB`, `BLOB`

#### Logical Database Structure

- **Entity Relationship Diagram (ERD)**:
  - **Definition**: ERD is a visual representation of the logical structure of a database, showing how entities are related.
  - **Cardinality of Relationships**:
    - **One-to-One**: One instance of an entity is related to one instance of another entity.
    - **One-to-Many**: One instance of an entity is related to multiple instances of another entity.
    - **Many-to-Many**: Multiple instances of an entity are related to multiple instances of another entity.

  - **Example ERD**:
  ![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/vcV56ybUT9yFeesm1N_c5Q_d92001d99fd4480da65a9786af328aa1_C1M1L4-Item-03---Image2.png?expiry=1717545600000&hmac=-jVEbb8tE9tFGPuKS0bh5u2E6INQOjnHuwNfdV-UFh4)
    - Depicts entities like `Student`, `Department`, `Course`.
    - Shows relationships such as `Student` enrolled in `Course`.

#### Physical Database Structure

- **Implementation**:
  - **Tables**: Entities from the ERD are implemented as tables.
  - **Relationships**: Established using foreign keys.

  - **Example**:
  ![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/MsMWk-7kR7yDFpPu5Oe8wg_f1ba53697efc4fd68d6107df2f8479a1_C1M1L4-Item-03---Image3.png?expiry=1717545600000&hmac=WhGkGW3kLuHgr2dvhYk_m93NMYrmO0zcl8gGJirqc8I)
    - Two tables: `Student` and `Department`.
    - `Student` table has a primary key `StudentID`.
    - `Department` table has a foreign key `StudentID` referencing `StudentID` in `Student` table.

### Summary

- **Tables (Entities)**: Store data in rows and columns.
- **Attributes (Fields)**: Describe properties of entities.
- **Records (Rows)**: Individual data entries in tables.
- **Primary Key**: Uniquely identifies each record.
- **Foreign Key**: Establishes relationships between tables.
- **Data Types**: Define the type of data that can be stored in each column.
- **Logical Structure (ERD)**: Visual representation of entities and relationships.
- **Physical Structure**: Actual implementation of tables and relationships in a DBMS.

By mastering these basic components and concepts, you can design and manage relational databases effectively, ensuring data is organized, consistent, and easily accessible.

# Types of keys in a database table

### Understanding Keys in Relational Databases

To fully grasp how a relational database model works, it is essential to understand the relationships between tables, which are established through the use of keys. By the end of this guide, you will be able to identify the main keys used in tables within a relational database and explain how they relate to one another.

#### Key Concepts in Relational Databases

- **Entities (Tables)**: Represent different objects or concepts, such as `League`, `Teams`, and `Points` in a sports competition database.
- **Relations**: Connections between tables using keys.

#### Example Scenario: Sports Competition

Consider a sports competition with three tables:

1. **League Table**: Tracks each team's position, name, and state.
2. **Teams Table**: Tracks team name, captain, and coach.
3. **Points Table**: Records team's league position, name, and points.

Notice that `team_name` appears in both the `League` and `Teams` tables, establishing a relationship between them.

#### Types of Attributes

- **Simple Attribute**: Holds a single value (e.g., `staff_name`).
- **Multi-value Attribute**: Holds multiple values but should be avoided in relational database design.

#### Types of Keys

1. **Primary Key**:
   - **Definition**: Uniquely identifies each record in a table.
   - **Example**: `staff_id` in the `Staff` table.

2. **Candidate Key**:
   - **Definition**: Any attribute that can uniquely identify a record.
   - **Example**: Both `staff_id` and `contact_number` in the `Staff` table.

3. **Composite Key**:
   - **Definition**: Formed by combining two or more attributes to create a unique identifier.
   - **Example**: Combination of `staff_name` and `staff_title` in the `Staff` table.

4. **Alternate Key (Secondary Key)**:
   - **Definition**: A candidate key not chosen as the primary key.
   - **Example**: `contact_number` in the `Staff` table.

5. **Foreign Key**:
   - **Definition**: An attribute that references the primary key in another table.
   - **Example**: `staff_id` in a `Courses` table that refers to `staff_id` in the `Staff` table.

#### Relationships Between Keys

- **Primary Key**: Ensures that each record within a table is unique.
- **Foreign Key**: Establishes a link between two tables, ensuring referential integrity.
  - **Example**: `student_id` in the `Courses` table referring to `student_id` in the `Students` table.

#### Practical Application: Staff Table Example

Consider a `Staff` table in a college database:

- **Primary Key**: `staff_id` (unique for each staff member).
- **Candidate Keys**: `staff_id` and `contact_number` (both unique for each record).
- **Composite Key**: Could be a combination of `staff_name` and `staff_title` if it uniquely identifies each record.
- **Alternate Key**: `contact_number` if `staff_id` is chosen as the primary key.
- **Foreign Key**: `staff_id` in related tables, such as a `Courses` table, to establish a relationship.

### Summary

- **Entities (Tables)**: Store data in rows and columns.
- **Attributes (Fields)**: Describe the properties of entities.
- **Keys**: Ensure data integrity and establish relationships between tables.
  - **Primary Key**: Uniquely identifies records.
  - **Candidate Key**: Potential unique identifiers.
  - **Composite Key**: Combines multiple attributes to create a unique identifier.
  - **Alternate Key**: Candidate key not chosen as the primary key.
  - **Foreign Key**: References primary key in another table to establish relationships.

By understanding these key attributes and their roles in a relational database, you can effectively design and manage databases, ensuring data integrity and meaningful relationships between different entities.

# Additional resources
The following resources are reading material that provide additional knowledge on database design and Relational Database structure.

- Microsoft
(https://support.microsoft.com/en-us/office/database-design-basics-eb2159cf-1e30-401a-8084-bd4f9c9ca1f5)

- IBM
(https://www.ibm.com/docs/en/control-desk/7.6.0?topic=design-relational-database-structure)