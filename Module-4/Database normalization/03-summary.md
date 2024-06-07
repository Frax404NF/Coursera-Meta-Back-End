# What is database normalization?

### Database Normalization and Anomalies

Database normalization is a crucial process in designing relational databases to minimize redundancy, reduce data anomalies, and improve data integrity. It involves organizing tables and their relationships to ensure that each table serves a single purpose and eliminates unnecessary duplication of data. Let's explore the concept of normalization and how it helps to address common issues like insert, update, and deletion anomalies.

#### Anomalies in Unnormalized Databases

Anomalies are problems that arise when we try to insert, update, or delete data in a poorly designed database. These anomalies can lead to data inconsistency and redundancy. The three main types of anomalies are:

1. **Insert Anomaly**
2. **Update Anomaly**
3. **Deletion Anomaly**

Let's use the example of a College Enrollment Table to understand these anomalies.

##### Example: College Enrollment Table
A table that combines students, courses, and departments might look like this:

| StudentID | StudentName | CourseID | CourseName | DeptID | DeptName | DeptHead  |
|-----------|-------------|----------|------------|--------|----------|-----------|
| 1         | Alice       | C101     | Math       | D1     | Science  | Dr. Smith |
| 2         | Bob         | C102     | Physics    | D1     | Science  | Dr. Smith |
| 3         | Carol       | C101     | Math       | D1     | Science  | Dr. Smith |

This table has several issues:
- **Data redundancy**: The department information is repeated for each student enrolled in courses offered by that department.
- **Insert anomaly**: You cannot add a new course without enrolling at least one student in it.
- **Update anomaly**: Changing the department head's name requires updating multiple rows.
- **Deletion anomaly**: Deleting a student record may also remove essential information about the course and department if no other students are enrolled in that course.

#### Database Normalization

Normalization addresses these anomalies by decomposing the table into smaller, related tables, each serving a specific purpose. The most commonly used normalization forms are the first, second, and third normal forms (1NF, 2NF, and 3NF).

1. **First Normal Form (1NF)**
2. **Second Normal Form (2NF)**
3. **Third Normal Form (3NF)**

Let's normalize the College Enrollment Table.

##### Step 1: First Normal Form (1NF)
1NF ensures that the table has a primary key and that each column contains atomic (indivisible) values.

```plaintext
STUDENT Table:
| StudentID | StudentName |
|-----------|-------------|
| 1         | Alice       |
| 2         | Bob         |
| 3         | Carol       |

COURSE Table:
| CourseID | CourseName | DeptID |
|----------|------------|--------|
| C101     | Math       | D1     |
| C102     | Physics    | D1     |

DEPARTMENT Table:
| DeptID | DeptName | DeptHead  |
|--------|----------|-----------|
| D1     | Science  | Dr. Smith |

ENROLLMENT Table:
| StudentID | CourseID |
|-----------|----------|
| 1         | C101     |
| 2         | C102     |
| 3         | C101     |
```

##### Step 2: Second Normal Form (2NF)
2NF ensures that the table is in 1NF and that all non-key attributes are fully functional dependent on the primary key. In our example, the tables already comply with 2NF since there are no partial dependencies.

##### Step 3: Third Normal Form (3NF)
3NF ensures that the table is in 2NF and that all attributes are functionally dependent only on the primary key. There are no transitive dependencies in the current tables.

#### Benefits of Normalization

- **Eliminates Redundancy**: Data is stored only once, reducing the storage requirement.
- **Minimizes Insert Anomaly**: New data can be inserted without requiring unrelated data.
- **Minimizes Update Anomaly**: Updating a single piece of information in one place.
- **Minimizes Deletion Anomaly**: Deleting a record does not result in unintended data loss.

#### Summary

Database normalization is essential to design efficient, reliable, and scalable databases. By structuring tables to eliminate redundancy and dependencies, normalization helps resolve common data anomalies, making data management easier and more consistent.

### Practical Example with SQL

Here is how you can create the normalized tables in SQL:

```sql
CREATE DATABASE CollegeEnrollment;
USE CollegeEnrollment;

CREATE TABLE Student (
    StudentID INT PRIMARY KEY,
    StudentName VARCHAR(50)
);

CREATE TABLE Course (
    CourseID VARCHAR(10) PRIMARY KEY,
    CourseName VARCHAR(50),
    DeptID VARCHAR(10)
);

CREATE TABLE Department (
    DeptID VARCHAR(10) PRIMARY KEY,
    DeptName VARCHAR(50),
    DeptHead VARCHAR(50)
);

CREATE TABLE Enrollment (
    StudentID INT,
    CourseID VARCHAR(10),
    PRIMARY KEY (StudentID, CourseID),
    FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Course(CourseID)
);
```

With these normalized tables, you can efficiently manage student enrollments, courses, and departments without encountering the typical anomalies of unnormalized tables.

# Data normalization

### Data Normalization: Understanding 1NF, 2NF, and 3NF

Data normalization is a fundamental process in database design that aims to minimize data duplication, avoid modification anomalies, and simplify data queries. The three primary normalization forms are the First Normal Form (1NF), Second Normal Form (2NF), and Third Normal Form (3NF). Let's explore how to apply these normalization rules using a practical example.

#### Example Scenario: Medical Group Surgery in London

Consider a Medical Group Surgery based in London with data about doctors, patients, surgeries, and appointments. Initially, the data might look like this in an unnormalized form:

| DoctorID | DoctorName | Region      | PatientID | PatientName | SurgeryNumber | Council | Postcode | SlotID | TotalCost |
|----------|------------|-------------|-----------|-------------|---------------|---------|----------|--------|-----------|
| D1       | Karl       | West London | P1        | Rami        | 3             | Harrow  | HA9SDE   | A1     | 1500      |
| D1       | Karl       | West London | P2        | Kim         | 3             | Harrow  | HA9SDE   | A2     | 1200      |
| D1       | Karl       | West London | P3        | Nora        | 3             | Harrow  | HA9SDE   | A3     | 1600      |
| D1       | Karl       | East London | P4        | Kamel       | 4             | Hackney | E1 6AW   | A1     | 2500      |
| D1       | Karl       | East London | P5        | Sami        | 4             | Hackney | E1 6AW   | A2     | 1000      |
| D2       | Mark       | East London | P5        | Sami        | 4             | Hackney | E1 6AW   | A3     | 1500      |
| D2       | Mark       | East London | P6        | Norma       | 4             | Hackney | E1 6AW   | A4     | 2000      |
| D2       | Mark       | West London | P7        | Rose        | 5             | Harrow  | HA862E   | A4     | 1000      |
| D2       | Mark       | West London | P1        | Rami        | 5             | Harrow  | HA862E   | A5     | 1500      |

### First Normal Form (1NF)

To achieve 1NF, ensure that each column contains atomic values, and there are no repeating groups or arrays.

1. **Identify repeating groups**: Multiple patients and slot IDs for each doctor.
2. **Create separate tables for entities with repeating groups**: Split the data into patient-specific, doctor-specific, and surgery-specific tables.

**Patient Table:**

| PatientID | PatientName | SlotID | TotalCost |
|-----------|-------------|--------|-----------|
| P1        | Rami        | A1     | 1500      |
| P2        | Kim         | A2     | 1200      |
| P3        | Nora        | A3     | 1600      |
| P4        | Kamel       | A1     | 2500      |
| P5        | Sami        | A2     | 1000      |
| P6        | Norma       | A4     | 2000      |
| P7        | Rose        | A5     | 1000      |

**Doctor Table:**

| DoctorID | DoctorName |
|----------|------------|
| D1       | Karl       |
| D2       | Mark       |

**Surgery Table:**

| SurgeryNumber | Region      | Council | Postcode |
|---------------|-------------|---------|----------|
| 3             | West London | Harrow  | HA9SDE   |
| 4             | East London | Hackney | E1 6AW   |
| 5             | West London | Harrow  | HA862E   |

**SQL for creating 1NF tables:**

```sql
CREATE TABLE Patient (
    PatientID VARCHAR(10) NOT NULL,
    PatientName VARCHAR(50),
    SlotID VARCHAR(10) NOT NULL,
    TotalCost DECIMAL,
    PRIMARY KEY (PatientID, SlotID)
);

CREATE TABLE Doctor (
    DoctorID VARCHAR(10) PRIMARY KEY,
    DoctorName VARCHAR(50)
);

CREATE TABLE Surgery (
    SurgeryNumber INT PRIMARY KEY,
    Region VARCHAR(20),
    Council VARCHAR(20),
    Postcode VARCHAR(10)
);
```

### Second Normal Form (2NF)

To achieve 2NF, eliminate partial dependencies. This applies to tables with composite primary keys, ensuring non-key attributes depend on the entire key.

1. **Identify composite keys**: In the Patient table, the composite key is (PatientID, SlotID).
2. **Remove partial dependencies**: Split the Patient table into separate tables for patients and appointments.

**Patient Table:**

| PatientID | PatientName |
|-----------|-------------|
| P1        | Rami        |
| P2        | Kim         |
| P3        | Nora        |
| P4        | Kamel       |
| P5        | Sami        |
| P6        | Norma       |
| P7        | Rose        |

**Appointments Table:**

| AppointmentID | PatientID | SlotID | TotalCost |
|---------------|-----------|--------|-----------|
| 1             | P1        | A1     | 1500      |
| 2             | P2        | A2     | 1200      |
| 3             | P3        | A3     | 1600      |
| 4             | P4        | A1     | 2500      |
| 5             | P5        | A2     | 1000      |
| 6             | P6        | A4     | 2000      |
| 7             | P7        | A5     | 1000      |

**SQL for creating 2NF tables:**

```sql
CREATE TABLE Patient (
    PatientID VARCHAR(10) PRIMARY KEY,
    PatientName VARCHAR(50)
);

CREATE TABLE Appointment (
    AppointmentID INT PRIMARY KEY,
    PatientID VARCHAR(10),
    SlotID VARCHAR(10),
    TotalCost DECIMAL,
    FOREIGN KEY (PatientID) REFERENCES Patient(PatientID)
);
```

### Third Normal Form (3NF)

To achieve 3NF, eliminate transitive dependencies. Ensure non-key attributes do not depend on other non-key attributes.

1. **Identify transitive dependencies**: In the Surgery table, Postcode depends on Council.
2. **Remove transitive dependencies**: Split the Surgery table into separate tables for surgery locations and councils.

**Location Table:**

| SurgeryNumber | Postcode |
|---------------|----------|
| 3             | HA9SDE   |
| 4             | E1 6AW   |
| 5             | HA862E   |

**Council Table:**

| Council | Region      |
|---------|-------------|
| Harrow  | West London |
| Hackney | East London |

**SQL for creating 3NF tables:**

```sql
CREATE TABLE Location (
    SurgeryNumber INT PRIMARY KEY,
    Postcode VARCHAR(10)
);

CREATE TABLE Council (
    Council VARCHAR(20) PRIMARY KEY,
    Region VARCHAR(20)
);
```

### Summary

By applying normalization rules, the original unnormalized table has been decomposed into multiple tables in 3NF, ensuring minimal redundancy, elimination of anomalies, and simplified queries. Here are the final table structures:
![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/sjd-BX3bSjS3fgV92wo0Xg_00cf0ffa83b84cdfba93e7087db297a1_C1M4L3I2-Image2-2.png?expiry=1717891200000&hmac=PjeGYvzwFQzdxs7kiYxxVFfqSvXr8YiSC9PmaN4hIcw)


![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/APRvV82LTZa0b1fNi62Wbg_95f32dfc0b974e2093d539a2b1f5afa1_C1M4L3I2-Image3.PNG?expiry=1717891200000&hmac=CgkgtaJehbF7gmFt5VyZfM0qlfoDf5z6tF3yaQlJBCA)
1. **Patient Table**
2. **Appointment Table**
3. **Doctor Table**
4. **Surgery Table**
5. **Location Table**
6. **Council Table**

These tables are now well-organized, with clear relationships and reduced complexity, ensuring the database is efficient and reliable.

# First normal form 1NF
As a database engineer, you often encounter columns with duplicate or multiple values, making it hard to manage data. **Normalization** helps address this issue. By the end of this guide, you'll understand how to design a database in **First Normal Form (1NF)**, enforce the **atomicity rule**, and eliminate repeating data.

**Normalization** improves efficiency by fixing **insert, delete, and update anomalies**. There are three main forms of normalization:
- **First Normal Form (1NF)**
- **Second Normal Form (2NF)**
- **Third Normal Form (3NF)**

This guide focuses on **1NF**, which ensures:
1. **Data Atomicity:** Each field contains only one value.
2. **Elimination of Repeating Groups:** Avoid redundant data.

### Example of Achieving 1NF

Consider an unnormalized **Course Table** with columns for course details and tutor contact numbers. If a column has multiple values (e.g., two phone numbers), it violates **1NF**.

**Steps to Normalize:**
1. **Identify Repeating Groups:** Tutor names and contact numbers.
2. **Identify Entities:** Separate **Course** and **Tutor** entities.
3. **Split the Table:** Create a **Course Table** and a **Tutor Table**.
4. **Assign Primary Keys:** Use **Tutor ID** as the primary key in the Tutor Table.
5. **Link Tables with Foreign Keys:** Add **Tutor ID** as a foreign key in the Course Table.

By following these steps, you achieve **data atomicity** and eliminate unnecessary repeating groups, ensuring a well-structured and efficient database design.

### Summary of Benefits:
- **Increased Data Integrity:** Consistent and accurate data.
- **Reduced Redundancy:** No repeated data across tables.
- **Improved Efficiency:** Easier data management and operations.

# Second normal form 2NF

### Summary of Designing a Database in Second Normal Form (2NF)

**Database normalization** is an important process in database design that aims to reduce data redundancy and improve data integrity. Here’s a simplified explanation and detailed summary:

#### **Importance of Database Normalization**

- **Reduces data duplication**: By organizing tables efficiently, we minimize repeated data.
- **Enhances data accuracy**: Properly structured databases allow for more accurate data retrieval and analysis.

#### **Understanding First Normal Form (1NF)**
Before moving to 2NF, you must ensure your database is in **First Normal Form (1NF)**, where:
- All columns contain atomic (indivisible) values.
- Each column contains values of a single type.
- Each column has a unique name.
- The order in which data is stored does not matter.

#### **Second Normal Form (2NF) Criteria**
To design a database in **Second Normal Form (2NF)**:
1. **Meet all 1NF criteria**.
2. **Eliminate partial dependencies**: Non-key attributes must depend on the entire primary key, not just part of it.

#### **Concepts: Functional and Partial Dependency**

- **Functional Dependency**: When the value of one attribute (e.g., primary key) uniquely determines another attribute’s value. For example, in a **Student table**, the **student ID** (primary key) uniquely determines the student’s **name** and **date of birth**.
  
- **Partial Dependency**: Occurs in tables with composite keys, where a non-key attribute depends only on part of the primary key. This violates 2NF. For instance, in a **Vaccination table** with **patient ID** and **vaccine ID** as composite keys, if the **vaccine name** depends only on **vaccine ID**, it’s a partial dependency.

#### **Example: Upgrading a Table to 2NF**

Consider a **Vaccination table** with columns: **patient ID**, **vaccine ID**, **vaccine name**, **patient name**, and **status**.

**Step-by-Step Process**:
1. **Identify Entities**: Break the table into multiple tables based on entities:
   - **Patient Table**: Contains **patient ID** and **patient name**.
   - **Vaccine Table**: Contains **vaccine ID** and **vaccine name**.
   - **Vaccination Status Table**: Contains **patient ID**, **vaccine ID**, and **status**.

2. **Ensure Full Dependency**: In each table, all non-key attributes must depend on the entire primary key.
   - In the **Patient Table**, **patient name** depends on **patient ID**.
   - In the **Vaccine Table**, **vaccine name** depends on **vaccine ID**.
   - In the **Vaccination Status Table**, **status** depends on the combination of **patient ID** and **vaccine ID**.

By restructuring the **Vaccination table** into three separate tables, we eliminate partial dependencies and achieve 2NF.

#### **Conclusion**

- **Second Normal Form (2NF)** ensures that all non-key attributes are fully dependent on the entire primary key.
- This reduces redundancy and improves data integrity in your database.
- Understanding and applying **functional** and **partial dependencies** is crucial for achieving 2NF.

By following these principles, you can optimize your database structure, making it easier to manage, search, and analyze data accurately.

# Third normal form 3NF

### Summary of Designing a Database in Third Normal Form (3NF)

**Database normalization** is an essential process in designing efficient relational databases, and the third normal form (3NF) is a critical step in this process. Here’s a simplified explanation and detailed summary:

#### **Importance of Third Normal Form (3NF)**

- **Eliminates transitive dependencies**: Ensures that non-key attributes do not depend on other non-key attributes, reducing redundancy and improving data integrity.

#### **Prerequisites**

- The database must already be in **First Normal Form (1NF)** and **Second Normal Form (2NF)**:
  - **1NF**: All columns contain atomic values and each column contains values of a single type.
  - **2NF**: Eliminates partial dependencies where non-key attributes depend on the entire primary key.

#### **Understanding Third Normal Form (3NF)**

**Third Normal Form (3NF)** requires:
1. **Meet all 2NF criteria**.
2. **Eliminate transitive dependencies**: A non-key attribute cannot be functionally dependent on another non-key attribute.

#### **Concept: Transitive Dependency**

- **Transitive Dependency**: Occurs when a non-key attribute depends on another non-key attribute. For example, if **A** determines **B** and **B** determines **C**, then **A** determines **C** through **B**.

#### **Example: Identifying Transitive Dependencies**

Consider a **Books table** with the following columns:
- **Book ID** (primary key)
- **Title**
- **Author Name**
- **Author Language**
- **Country**

In this table:
- **Book ID** uniquely determines all other attributes.
- However, **Author Language** and **Country** are interdependent non-key attributes. Knowing the language can determine the country and vice versa.

#### **Steps to Achieve 3NF**

1. **Identify Transitive Dependencies**: Determine which non-key attributes depend on other non-key attributes. In the **Books table**, **Language** and **Country** are transitively dependent.

2. **Split the Table**: Separate the attributes into different tables to eliminate transitive dependencies.
   - Create a new **Country table** with **Country** and **Language**.
   - Leave the **Country** column in the original **Books table** as a foreign key to connect with the **Country table**.

#### **Detailed Example**

**Initial Table:**
- **Books table**: Book ID, Title, Author Name, Author Language, Country

**Reorganized Tables to Achieve 3NF:**
1. **Books table**:
   - **Book ID** (primary key)
   - **Title**
   - **Author Name**
   - **Country** (foreign key)

2. **Country table**:
   - **Country** (primary key)
   - **Language**

By splitting the **Books table** and creating a **Country table**, we eliminate the transitive dependency between **Language** and **Country**.

#### **Benefits of 3NF**

- **Reduces redundancy**: By ensuring that non-key attributes only depend on primary keys, we avoid repetitive data.
- **Improves data integrity**: Changes in one table do not affect unrelated data in another table, making the database more consistent and reliable.

#### **Conclusion**

- **Third Normal Form (3NF)** ensures that all non-key attributes are directly dependent on the primary key, eliminating transitive dependencies.
- By following the principles of 3NF, you can optimize your database structure, reduce redundancy, and improve data integrity.

Understanding and implementing 3NF is crucial for creating efficient and well-structured relational databases.

# Module summary: Database design
## Database Design Module Recap

This module covered the fundamentals of database design, focusing on relational databases and normalization techniques. Here's a breakdown of the key areas you learned:

**1. Database Schema:**

* Definition: The blueprint or structure of a database, outlining how data is organized.
* Importance: A well-designed schema is crucial for efficient data storage, retrieval, and analysis.
* Creation using SQL: Basic commands allow you to build a schema using tables, columns, and data types.
* Types: Logical schema describes the overall data structure, while the physical schema specifies how data is physically stored.

**2. Relational Database Design:**

* Relational Model: Organizes data into tables with relationships between them.
* Types of Relations: One-to-one, one-to-many, and many-to-many relationships connect tables.
* Entity-Relationship Diagram (ERD): A visual representation of entities, attributes, and relationships.
* Primary Key: A unique identifier for each row in a table, ensuring data integrity.
    * Selecting a primary key: Can be a single column or a combination of columns (composite key).
* Foreign Key: References a primary key in another table, establishing connections.
* Entities: Represent real-world objects or concepts with attributes describing their properties.

**3. Database Normalization:**

* Process: Dividing a large table into smaller, more efficient tables with minimal redundancy.
* Anomalies: Data inconsistencies that can occur in unnormalized databases (insertion, deletion, update).
* Normalization Forms:
    * First Normal Form (1NF): Eliminates repeating groups of data within a table.
    * Second Normal Form (2NF): Ensures all non-key attributes depend on the full primary key, not just a part of it.
    * Third Normal Form (3NF): Removes transitive dependencies, where one non-key attribute depends on another non-key attribute.
* Key Concepts: Functional, partial, and transitive dependencies describe relationships between attributes.

By understanding these concepts and practicing normalization techniques, you can create well-structured relational databases that are efficient and minimize data redundancy. This paves the way for accurate data storage, retrieval, and analysis, ultimately supporting your learning goals.

# Additional resources
The following resources introduce key concepts of database design and anomalies. They also provide good examples of first, second and third normal forms.

- Database Design – 2nd Edition
https://opentextbc.ca/dbdesign01/chapter/chapter-12-normalization/

- Database Normalization
https://www.databasestar.com/database-normalization/

- Anomalies
https://www.bbc.co.uk/bitesize/guides/zc93tv4/revision/2
