# Table relationships

Sure, let's break down the concepts and examples mentioned in your prompt to better understand the relational model for databases, how it influences database design and structure, and how to extract information from a well-structured database. We will also touch on the basics of an Entity-Relationship Diagram (ERD).

### Basics of the Relational Model

The relational model organizes data into tables (or relations). Each table has rows (or records) and columns (or attributes). The relational model is based on the concept of keys and relationships between tables.

### Types of Relationships

There are three main types of relationships between tables in a relational database:

1. **One-to-Many (1:M) Relationship**:
    - **Example**: A student is enrolled in multiple courses.
    - **Illustration**: 
        - **Student Table**: Contains student details and a unique student ID.
        - **Course Table**: Contains course details and a unique course ID.
        - **Relationship**: A student (with a unique student ID) can be linked to multiple courses (each course with a unique course ID). This is often depicted in an ERD where a single student entity is connected to multiple course entities using the Crow's foot notation to represent "many".
        - **Keys**: The course ID in the student table is a foreign key (FK) that references the primary key (PK) course ID in the course table.

2. **One-to-One (1:1) Relationship**:
    - **Example**: Each department head is associated with one department.
    - **Illustration**: 
        - **Department Staff Table**: Contains staff details and a unique staff ID.
        - **Department Location Table**: Contains department location details and a unique department ID.
        - **Relationship**: A single department head (staff ID) is linked to a single department location (department ID). In an ERD, this is depicted with a single line connecting the two entities without any additional notation since both ends represent "one".
        - **Keys**: The department ID in the department staff table is a foreign key (FK) referencing the primary key (PK) in the department location table.

3. **Many-to-Many (M:N) Relationship**:
    - **Example**: Students working on multiple research projects, supervised by different staff members.
    - **Illustration**: 
        - **Student Table**: Contains student details and a unique student ID.
        - **Project Table**: Contains project details and a unique project ID.
        - **Staff Table**: Contains staff details and a unique staff ID.
        - **Relationship**: A student can work on multiple projects and each project can be supervised by multiple staff members. This is depicted in an ERD using a diamond shape labeled "supervised by" connecting students and staff with a Crow's foot notation at both ends to indicate "many".
        - **Keys**: Typically involves a junction table (e.g., Student_Project) with foreign keys referencing the primary keys of both student and project tables.

### Entity-Relationship Diagram (ERD)

An ERD is a visual representation of the entities in a database and the relationships between them. It includes:
- **Entities**: Represented by rectangles (e.g., Student, Course, Staff, Department).
- **Relationships**: Represented by diamonds (e.g., enrolled in, supervised by).
- **Attributes**: Represented by ovals and connected to their respective entities.
- **Keys**: Primary keys (PK) are underlined, and foreign keys (FK) are noted.

### Extracting Information

Understanding the relational model helps in querying the database efficiently. For example:
- **SQL Queries**: To find out which student is studying which course, you can join the Student table with the Course table on the foreign key relationship.

```sql
SELECT Student.name, Course.course_name
FROM Student
JOIN Enrollment ON Student.student_id = Enrollment.student_id
JOIN Course ON Enrollment.course_id = Course.course_id;
```

This SQL query joins the student and course tables through an enrollment table that holds the foreign keys, allowing us to see which students are enrolled in which courses.

### Conclusion

The relational model helps in structuring the database effectively, enabling efficient data retrieval and ensuring data integrity through defined relationships. ER diagrams provide a clear and visual way to represent and understand these relationships, making it easier to design and maintain a relational database.

# Relational model

### Relational Model: An Overview

**The relational database model** is a foundational concept in database systems that organizes data into tables, which are related to each other. Despite its age, it remains the most widely used model for commercial databases due to its robustness and efficiency.

### What is the Relational Model?

**The relational model** is based on three main concepts:
1. **Data**
2. **Relationships**
3. **Constraints**

It describes a database as **"a collection of inter-related relations (or tables)"**. Essentially, it organizes and stores data in a database where **SQL** is the language used to retrieve this data.

### Fundamental Concepts of the Relational Model

- **Relation**: Represents a file that stores data, also known as a **table**. A table consists of **rows (tuples/records)** and **columns (fields/attributes)**. Each row represents a group of related data values.
  
- **Column**: The principal storage unit of a database. Each column represents a piece of data to be stored and has a specific data type (e.g., numeric, text).

- **Domain**: A set of acceptable values that a column can contain, depending on its data type. For example, an ID column can only contain numeric values.

- **Record (Tuple)**: A row within a table. For instance, a record in a student table would contain a student's ID, first name, and last name.

- **Key**: An attribute or a set of attributes that can uniquely identify a specific row, known as the **primary key**.

- **Degree**: The number of columns or attributes in a relation. For example, a student table with columns for name, address, phone number, and email address has a degree of four.

- **Cardinality**: The number of records within a table. If a student table has 100 students, its cardinality is 100.

### Constraints in the Relational Model

For a relation to be valid, it must meet three conditions known as **relational integrity constraints**:

1. **Key Constraints**: Each record must have a unique key attribute. For example, a Student ID must be unique for each student.

2. **Domain Constraints**: Values in each column must adhere to the specified data type. For instance, a numeric column cannot contain text.

3. **Referential Integrity Constraints**: These ensure that a foreign key in one table correctly references a primary key in another table. For example, an Order_Item table may have an order ID that references the primary key in the Order table.

### Types of Relationships

In the relational model, relationships between tables can be:

- **One-to-One**: Each record in Table A relates to one record in Table B, and vice versa. For example, each country has one capital.

- **One-to-Many**: A record in Table A can relate to zero, one, or many records in Table B. For example, one customer can place many orders.

- **Many-to-Many**: Many records in Table A can relate to many records in Table B. This is often managed by breaking it into two one-to-many relationships with a junction table.

### Conclusion

The relational database model offers several benefits, such as the ability to design and develop a meaningful system of information and the ability to efficiently access and retrieve data. Its structure, based on tables and defined relationships, ensures data integrity and ease of use, making it a preferred choice for database management systems.

# Primary key

### Primary Keys in Databases: Simplified Explanation

When working with databases, querying specific records can become challenging if there are duplicates. **Keys** provide a solution to this problem by uniquely identifying each record.

### What is a Primary Key?

A **primary key** is a unique identifier for a record in a database table. It ensures that each record can be uniquely identified, which helps prevent duplicates.

#### Example: Student Table

Consider a student table with the following attributes:
- **ID**
- **Name**
- **Date of Birth**
- **Email**
- **Grade**

To uniquely identify a student (e.g., to enter their grade), we need a unique attribute. Let's look at the student "Mary" on row 2:

- **Name**: There could be multiple students named Mary.
- **Date of Birth**: Another student named Dan might share the same birthday.

Neither the **name** nor the **date of birth** can uniquely identify Mary. Thus, we need a **candidate key**, an attribute that is unique for each row and cannot be null.

### Identifying Candidate Keys

In this example, two possible candidate keys are:
- **Student ID**: Each student has a unique ID.
- **Student Email**: Each student has a unique email.

We can choose either as the primary key. If we select **Student ID** as the primary key, **Student Email** becomes the secondary key.

### What if No Unique Column Exists?

Sometimes, no single column contains unique values. In such cases, we use a **composite primary key**, which is a combination of two or more attributes.

#### Example: Delivery Table

Consider a delivery department's table that tracks customer orders:
- **Customer ID**
- **Product Code**
- **Delivery Date**

No single column is unique. We can create a unique identifier by combining:
- **Customer ID**
- **Product Code**

This combination forms a **composite primary key**, uniquely identifying each record and allowing us to track deliveries.

### Types of Primary Keys

- **Single Column Primary Key**: Uses one attribute to uniquely identify records. 
  - Example: **Student ID** in a student table.
  
- **Composite Primary Key**: Combines multiple attributes to create a unique identifier.
  - Example: **Customer ID** and **Product Code** in a delivery table.

### Summary

Primary keys are essential for uniquely identifying records in a database. They prevent duplicates and ensure data integrity. Depending on the table structure and the data, you can use either a single column primary key or a composite primary key to maintain uniqueness.

- **Primary Key**: Unique identifier for a record.
- **Candidate Key**: A unique attribute that can be a primary key.
- **Secondary Key**: An alternate unique attribute.
- **Composite Primary Key**: A combination of attributes used when no single attribute is unique.

By understanding these concepts, you can efficiently manage and query data in database tables.

# Foreign key

### Foreign Keys in Relational Databases: Simplified Explanation

**Foreign keys** are crucial for connecting different tables in a relational database, enabling cross-referencing between them. Let's dive into the purpose and usage of foreign keys with a practical example involving a bookstore database.

### What is a Foreign Key?

**A foreign key** is one or more columns in a table that refer to the primary key columns of another table. This creates a relationship between the two tables, allowing for data integrity and accurate data retrieval.

### Example: Bookstore Database

Imagine a bookstore with two tables:
1. **Customer Table**: Tracks customer information (e.g., name, address).
2. **Order Table**: Tracks customer orders (e.g., order date, status).

**Problem**: How to determine which customer made which order?

**Solution**: Add a **Customer ID** column to the Order Table as a foreign key. This connects each order to the corresponding customer.

### How Foreign Keys Work

#### Establishing Relationships

- **Customer Table**: Contains **Customer ID** (primary key), **Name**, **Address**.
- **Order Table**: Contains **Order ID**, **Order Date**, **Status**, **Customer ID** (foreign key).

The **Customer ID** in the Order Table links back to the **Customer ID** in the Customer Table, creating a relationship.

### Parent and Child Tables

- **Parent Table**: The table containing the primary key being referenced (Customer Table).
- **Child Table**: The table containing the foreign key (Order Table).

**Example**:
- **Parent Table (Customer Table)**: Each customer record is unique.
- **Child Table (Order Table)**: Each order refers to one customer, but a customer can have multiple orders.

### One-to-Many Relationship

**One-to-Many Relationship**: Each customer can place many orders, but each order is associated with only one customer.

### Diagram Example

- **Customer Table**:
  - Customer ID (Primary Key)
  - Name
  - Address

- **Order Table**:
  - Order ID
  - Order Date
  - Status
  - **Customer ID** (Foreign Key)

The **Customer ID** in the Order Table points to the **Customer ID** in the Customer Table, linking each order to a specific customer.

### Multiple Foreign Keys

A table can have multiple foreign keys to establish relationships with multiple parent tables.

#### Example with Product Table

- **Customer Table**: Contains customer information.
- **Order Table**: Contains order information and has foreign keys:
  - **Customer ID** (links to Customer Table)
  - **Product ID** (links to Product Table)
  
- **Product Table**: Contains product information.

### Relationships

- **Customer and Order**: One-to-Many (Each customer can have many orders).
- **Order and Product**: One-to-One (Each order is for a specific product).

### Determining Parent and Child

In this setup:
- **Customer Table**: Parent
- **Product Table**: Parent
- **Order Table**: Child

The **Order Table** has two foreign keys, connecting it to both the **Customer Table** and the **Product Table**.

### Purpose of Foreign Keys

- **Maintain Data Integrity**: Ensures that each order is linked to a valid customer.
- **Enable Accurate Data Retrieval**: Allows for joining tables to fetch complete records (e.g., generating an invoice or delivering an order).

By using foreign keys, you create a robust relational database structure that ensures data integrity and simplifies data management.

### Summary

- **Foreign Key**: Connects tables by referring to primary key columns in another table.
- **Parent Table**: Contains the primary key.
- **Child Table**: Contains the foreign key.
- **One-to-Many Relationship**: Common relationship type where one record in the parent table relates to multiple records in the child table.
- **Multiple Foreign Keys**: Allows a table to be related to multiple parent tables, enhancing data integrity and relationship mapping.

Understanding and utilizing foreign keys effectively can significantly enhance the functionality and reliability of a relational database.

# Keys in depth
### Keys in Depth: Primary and Foreign Keys in Relational Databases

#### Understanding Keys in Relational Databases

In a relational database, keys are essential to identify records uniquely and establish relationships between tables. There are various types of keys, but two of the most critical ones are **primary keys** and **foreign keys**.

### Primary Keys

A **primary key** is a column or a set of columns in a table that uniquely identifies each row in that table. A table can have only one primary key, and it must contain unique values. The primary key ensures that no two rows have the same identifier.

#### Candidate Keys

Candidate keys are attributes that can potentially serve as a primary key. They must contain unique values in all rows and cannot have NULL values. Once you select a candidate key as a primary key, the remaining candidate keys become alternate keys.

**Example: Vehicle Table**

```plaintext
Vehicle ID  | Owner ID | Car Plate Number | Owner Phone Number
------------|----------|------------------|-------------------
D01         | Ow01     | PL02NY           | 0738297294
D02         | Ow02     | SN02L2           | 0725021582
D03         | Ow03     | PK02L2           | 0765021583
```

From the above table, the candidate keys are:
- Vehicle ID
- Car Plate Number
- Owner Phone Number

The **Owner ID** is not a candidate key because an owner can have multiple cars, leading to duplicate values.

The best choice for a primary key here is the **Vehicle ID** as it is unique and stable (not subject to change).

#### Creating the Vehicle Table with a Primary Key

To create the `vehicle` table in a database called `automobile` and set `vehicleID` as the primary key:

1. **Create the Database:**
   ```sql
   CREATE DATABASE automobile;
   ```

2. **Select the Database:**
   ```sql
   USE automobile;
   ```

3. **Create the Vehicle Table:**
   ```sql
   CREATE TABLE vehicle (
       vehicleID VARCHAR(10) PRIMARY KEY,
       ownerID VARCHAR(10),
       plateNumber VARCHAR(10),
       phoneNumber INT
   );
   ```

### Foreign Keys

A **foreign key** is a column or a set of columns in one table that refers to the primary key in another table. Foreign keys establish a relationship between two tables, enforcing referential integrity.

**Example: Owner Table**

```plaintext
Owner ID  | Owner Name     | Owner Address
----------|----------------|---------------
Ow01      | Amjad Omer     | 110, Elephant Way
Ow02      | Hans Henderson | 120, Dragon Way
Ow03      | Paulo Galdames | 130, Giraffe Avenue
```

The **Owner ID** column is the primary key in the `owner` table. We will use this `Owner ID` as a foreign key in the `vehicle` table to link each vehicle to its owner.

#### Creating the Owner Table with a Primary Key

1. **Create the Owner Table:**
   ```sql
   CREATE TABLE owner (
       ownerID VARCHAR(10) PRIMARY KEY,
       ownerName VARCHAR(50),
       ownerAddress VARCHAR(255)
   );
   ```

### Establishing the Relationship with Foreign Keys

To connect the `vehicle` table with the `owner` table using the `Owner ID` as a foreign key, you can alter the structure of the `vehicle` table:

1. **Add Foreign Key to Vehicle Table:**
   ```sql
   ALTER TABLE vehicle ADD FOREIGN KEY (ownerID) REFERENCES owner (ownerID);
   ```

### Understanding the Relationship

- **One-to-Many Relationship**: Each owner can have multiple vehicles, but each vehicle is associated with only one owner. This is a classic one-to-many relationship where the `owner` table is the parent, and the `vehicle` table is the child.

### Examining Table Structures

1. **Show Tables:**
   ```sql
   SHOW TABLES;
   ```

2. **Show Columns from Vehicle:**
   ```sql
   SHOW COLUMNS FROM vehicle;
   ```

### Key Types in MySQL

- **PRI**: Indicates a primary key.
- **UNI**: Indicates a unique key.
- **MUL**: Indicates that the column can contain multiple identical values (used in foreign keys).

By following these steps and understanding these concepts, you can effectively choose primary keys from candidate keys and establish relationships between tables using foreign keys in SQL. This ensures data integrity and enables complex queries across multiple tables.

# Finding entitites

### Identifying Entities and Attributes in a Relational Database

In designing a relational database, it's crucial to determine which tables (entities) and columns (attributes) to include. This process involves identifying the entities you need to maintain data on and understanding the different types of attributes associated with these entities.

#### What is an Entity?

An **entity** is an object or thing that has properties defining its characteristics. In the context of a relational database, an entity can represent anything such as a person, place, event, or object. Each entity is represented as a table in the database, and the table name often reflects the entity name.

For example:
- **Customer**: Represents individual customers.
- **Order**: Represents purchase orders made by customers.
- **Product**: Represents items available for sale.

#### Example of an Entity: Deliveries Table

Consider a table named **Deliveries** in an e-commerce database:
- **Table Name**: Deliveries (represents the entity)
- **Columns**: Each column represents an attribute of the entity such as `ID`, `CustomerName`, `DeliveryStatus`.

#### Types of Attributes

Attributes are the properties or characteristics of an entity. They can be categorized into different types:

1. **Simple Attributes**: These are indivisible attributes.
   - Example: `Grade` in a student table.

2. **Composite Attributes**: These attributes can be divided into smaller sub-attributes.
   - Example: `Name` can be split into `FirstName` and `LastName`.

3. **Single-Valued Attributes**: These attributes hold a single value for each record.
   - Example: `DateOfBirth` in a student table.

4. **Multi-Valued Attributes**: These attributes can hold multiple values for a single record (though typically avoided in relational databases).
   - Example: `Emails` could hold multiple email addresses.

5. **Derived Attributes**: These attributes derive their value from other attributes.
   - Example: `Age` can be derived from `DateOfBirth`.

6. **Key Attributes**: These are unique identifiers for each record in an entity.
   - Example: `StudentID` in a student table.

### Example: Student Table Attributes

Consider a **Student** table in a relational database. Here’s how different types of attributes might look:

- **Simple Attribute**: `Grade` (cannot be divided further)
- **Composite Attribute**: `Name` (can be split into `FirstName` and `LastName`)
- **Single-Valued Attribute**: `DateOfBirth` (only one value per student)
- **Multi-Valued Attribute**: `Email` (although better practice is to avoid multi-valued attributes by creating another table for emails)
- **Derived Attribute**: `Age` (derived from `DateOfBirth`)
- **Key Attribute**: `StudentID` (unique identifier)

### Creating Tables in SQL

#### Step-by-Step Instructions for Creating a Database and Tables

1. **Create the Database:**
   ```sql
   CREATE DATABASE school;
   USE school;
   ```

2. **Create the Student Table:**
   ```sql
   CREATE TABLE Student (
       StudentID VARCHAR(10) PRIMARY KEY,
       FirstName VARCHAR(50),
       LastName VARCHAR(50),
       DateOfBirth DATE,
       Grade CHAR(2),
       Email VARCHAR(100)
   );
   ```

3. **Create the Owner Table:**
   ```sql
   CREATE TABLE Owner (
       OwnerID VARCHAR(10) PRIMARY KEY,
       OwnerName VARCHAR(50),
       OwnerAddress VARCHAR(255)
   );
   ```

4. **Create the Vehicle Table and Add Foreign Key:**
   ```sql
   CREATE TABLE Vehicle (
       VehicleID VARCHAR(10) PRIMARY KEY,
       OwnerID VARCHAR(10),
       PlateNumber VARCHAR(10),
       PhoneNumber VARCHAR(15),
       FOREIGN KEY (OwnerID) REFERENCES Owner(OwnerID)
   );
   ```

### Understanding Relationships Between Tables

In a relational database, tables can be related using keys:

- **Primary Key**: Uniquely identifies each record in a table.
- **Foreign Key**: Establishes a link between two tables. It is a field (or collection of fields) in one table, that uniquely identifies a row of another table.

#### Example Relationship: Customer and Orders

- **Customer Table**: Contains customer details (with `CustomerID` as the primary key).
- **Order Table**: Contains order details and includes `CustomerID` as a foreign key to link each order to a specific customer.

```sql
-- Customer Table
CREATE TABLE Customer (
    CustomerID VARCHAR(10) PRIMARY KEY,
    CustomerName VARCHAR(50),
    CustomerAddress VARCHAR(255)
);

-- Order Table
CREATE TABLE Order (
    OrderID VARCHAR(10) PRIMARY KEY,
    OrderDate DATE,
    CustomerID VARCHAR(10),
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
);
```

In this example:
- **Customer** is the parent table.
- **Order** is the child table.

The foreign key ensures that each order is associated with a valid customer, enforcing referential integrity within the database.

### Summary

Identifying entities and attributes is crucial in designing an effective relational database. Understanding the types of attributes and how to use primary and foreign keys to establish relationships between tables ensures data integrity and supports complex queries across multiple tables. Always focus on including only the necessary entities and attributes that help users accomplish specific tasks and activities within your database system.

# Entity relationship diagrams (ERD)

Entity-Relationship Diagrams (ERDs) are essential tools in the design and documentation of relational databases. They help in visualizing the database structure, capturing data requirements, and defining the relationships between different data entities. ERDs serve as blueprints for database developers, guiding the implementation of the database in management systems like Oracle and MySQL.

#### Components of an ERD

1. **Entities**
2. **Attributes**
3. **Relationships**

### Entities

Entities represent objects or things in the database, such as people, places, or concepts. Each entity is depicted as a box in the ERD, with two compartments:
- The top compartment contains the entity name.
- The bottom compartment lists the related attributes.

**Example: College Enrollment System Entities**

- **Student**
- **Course**
- **Department**

### Attributes

Attributes are properties or characteristics of entities. Each attribute should be defined with a suitable data type.

**Attributes in the College Enrollment System:**

- **Student Entity**: `studentID`, `name`, `dateOfBirth`
- **Course Entity**: `courseID`, `courseName`, `courseCredits`
- **Department Entity**: `departmentNumber`, `departmentName`, `headOfDepartment`

### Relationships

Relationships define how entities are related to each other. The cardinality of the relationship (1:1, 1:N, M:N) determines the type of line used in the ERD.

- **1:1 (One-to-One)**: A straight line represents this relationship. Example: Each person has one passport.
- **1:N (One-to-Many)**: A straight line with a crow’s foot notation on one side represents this relationship. Example: One parent can have many children.
- **M:N (Many-to-Many)**: A straight line with crow’s foot notations on both sides represents this relationship. Example: Many students enroll in many courses.

### ERD Example: College Enrollment System

**Entities and Attributes:**

1. **Student**
   - studentID (Primary Key)
   - name
   - dateOfBirth
   - courseID (Foreign Key to Course)

2. **Course**
   - courseID (Primary Key)
   - courseName
   - courseCredits
   - departmentNumber (Foreign Key to Department)

3. **Department**
   - departmentNumber (Primary Key)
   - departmentName
   - headOfDepartment

**Relationships:**

1. **Student to Course**: One-to-Many (1:N)
   - Many students enroll in one course.
   - Depicted with a straight line and a crow’s foot notation at the course side.

2. **Course to Department**: One-to-Many (1:N)
   - Many courses belong to one department.
   - Depicted with a straight line and a crow’s foot notation at the department side.

### ERD Diagram for the College Enrollment System

```plaintext
+-------------------+    +------------------+    +----------------------+
|    Student        |    |     Course       |    |     Department       |
+-------------------+    +------------------+    +----------------------+
| studentID (PK)    |    | courseID (PK)    |    | departmentNumber (PK)|
| name              |    | courseName       |    | departmentName       |
| dateOfBirth       |    | courseCredits    |    | headOfDepartment     |
| courseID (FK)     |    | departmentNumber (FK) |                      |
+-------------------+    +------------------+    +----------------------+
       |                            |                             |
       |                            |                             |
       |                            |                             |
       |                            |                             |
       |                            |                             |
       +--------------------------->|                             |
(1:N Relationship)      (1:N Relationship)
```

In this ERD:
- Each **student** enrolls in one **course**, and each **course** can have many **students**.
- Each **course** is associated with one **department**, and each **department** can offer many **courses**.

### Steps to Create the Database

#### 1. Create the Database
```sql
CREATE DATABASE college_enrollment;
USE college_enrollment;
```

#### 2. Create the Tables

**Student Table**
```sql
CREATE TABLE Student (
    studentID VARCHAR(10) PRIMARY KEY,
    name VARCHAR(50),
    dateOfBirth DATE,
    courseID VARCHAR(10),
    FOREIGN KEY (courseID) REFERENCES Course(courseID)
);
```

**Course Table**
```sql
CREATE TABLE Course (
    courseID VARCHAR(10) PRIMARY KEY,
    courseName VARCHAR(50),
    courseCredits INT,
    departmentNumber VARCHAR(10),
    FOREIGN KEY (departmentNumber) REFERENCES Department(departmentNumber)
);
```

**Department Table**
```sql
CREATE TABLE Department (
    departmentNumber VARCHAR(10) PRIMARY KEY,
    departmentName VARCHAR(50),
    headOfDepartment VARCHAR(50)
);
```

### Summary

Entity-Relationship Diagrams (ERDs) are vital for visualizing and designing the structure of relational databases. They help in identifying entities, their attributes, and the relationships between them, ensuring that the database design is well-organized, efficient, and able to support the necessary operations and data integrity. Understanding how to create and interpret ERDs is fundamental for effective database development and management.

# Additional resources
The following list of resources covers the relational database model, clarifying its advantages and explaining how to design a relational database.

- Opentextbc1
https://opentextbc.ca/dbdesign01/chapter/chapter-8-entity-relationship-model/

- IBM
https://www.ibm.com/docs/en/ida/9.1.1?topic=entities-primary-foreign-keys

- Scaler
https://www.scaler.com/topics/dbms/relational-model-in-dbms/

- Oracle
https://www.oracle.com/database/what-is-a-relational-database/