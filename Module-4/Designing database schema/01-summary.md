# Database schema
#### **What is a Database Schema?**

- A **database schema** is a **blueprint** or **organization** of how data is stored in a database. It outlines the structure of the database and the relationships among the data.
- **General Definition**: It's a collection of data structures or an abstract design of how data is stored and organized.

#### **Schema Across Different Database Systems**

1. **MySQL**:
   - Schema and database are **interchangeable** terms.
   - A schema refers to the organization of data in the database and their relationships.

2. **SQL Server**:
   - A schema is a collection of components such as **tables, fields, datatypes, and keys**.

3. **PostgreSQL**:
   - A schema is a **namespace** that holds database objects like **views, indexes, and functions**.

4. **Oracle**:
   - A schema is assigned to each user, and each schema is named after its respective user.

#### **Key Concepts in All Database Systems**

- **Tables**: Represent the organizational units of data.
- **Relationships**: Define how tables are related to one another.

#### **Components of a Database Schema**

- **Schema Objects**: Include **tables, columns, relationships, datatypes, and keys**.
- **Example**: In a music database:
  - **Tables**: Artists, Albums, Genres.
  - **Relationships**: Tables are related through keys.

#### **Advantages of a Database Schema**

1. **Logical Groupings**:
   - Schemas provide a logical organization for database objects, making it easier to manage and understand the data structure.

2. **Ease of Access and Manipulation**:
   - Schemas make it more straightforward to access and manipulate database objects compared to other methods.

3. **Database Security**:
   - Schemas enhance security by allowing the granting of permissions to different users, protecting database objects based on user access rights.

4. **Ownership Transfer**:
   - Schemas facilitate the transfer of ownership of schema objects between users or other schemas.

### **Key Points**

- **A database schema** is essential for organizing and planning how data is stored and related in a database.
- **Different database systems** may have varying definitions and implementations of schemas, but they all serve the same fundamental purpose.
- **Schemas enhance security, organization, and ease of management** for database objects.

By understanding the concept of a **database schema**, you can better design, implement, and manage a database system, ensuring efficient and secure data storage and retrieval.

# Exploring database schema

### Exploring Database Schema

#### **What is a Database Schema?**

- **Database Schema**: A **blueprint** or structure of a database, outlining how data is organized, including tables, columns, rows, and relationships between tables.
- **Data Modeling**: The process of designing the database schema, usually done by database designers before any data is stored or manipulated.

#### **Three Main Types of Database Schema**

1. **Conceptual (Logical) Schema**:
    ![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/r0h7iPIeQpeIe4jyHiKXfg_fe02e3ef7cce4067aec2c3f003a49ae1_M4L1-Item-2-Reading_img-1.png?expiry=1717891200000&hmac=yZKFTVCUfMX-KvMt2bKaksdtSVOw8Xm953EDs4Afb1M)
   - **Defines**: The structure of the entire database, including entities, attributes, and relationships.
   - **Representation**: Often depicted using Entity-Relationship Diagrams (ERDs).
   - **Example**: An ERD showing employee and department entities and their relationships.
   - **Purpose**: Provides a high-level view of the database structure, abstracting physical storage details.

2. **Internal (Physical) Schema**:
    ![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/vIF5NqCGSeSBeTaghrnkug_4fdd78a7db9d4eee9d363f93d9c412e1_M4L1-Item-2-Reading_img-2.png?expiry=1717891200000&hmac=ShUSz7KwPPB6-fogXUe822-4T1K3YvvMvRy4g2KzwoQ)
   - **Defines**: How data is physically stored on disk, including tables, columns, and records.
   - **Representation**: Describes the actual storage and access paths.
   - **Example**: A diagram showing how the employee table is physically stored.
   - **Purpose**: Focuses on the low-level implementation details of data storage.

3. **External (View) Schema**:
    ![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/IgHfyxOYSAOB38sTmHgDhw_923eae8e86524b92939e589875b848e1_C1M3L1-Item-2-Image-03.png?expiry=1717891200000&hmac=72tdHp7YByOOvKll2e5sZa-5PqaJuKe3HT9BOWElCjk)
   - **Defines**: Different user views of the database, showing only relevant parts of the database to specific users.
   - **Representation**: Different schemas for different users, each showing a subset of the database.
   - **Example**: Different views of the employee table for various users like User1, User2, and User3.
   - **Purpose**: Tailors database views to meet the needs of different users or departments, hiding irrelevant details.

#### **Three-Schema Architecture**

- **Diagram**: A visual representation showing how conceptual, internal, and external schemas relate to each other.
![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/9yGyX4cuTgWhsl-HLv4FsQ_7201d84b45b045abba7a04b3fafd0ae1_M4L1-Item-2-Reading_img-4.png?expiry=1717891200000&hmac=Jue6lQnRKr-qjqYzaUZO3bBnGVNIT9ttzH4vxbSxsrQ)
- **Purpose**: Helps in understanding how data is abstracted and organized at different levels.

#### **Importance of Database Schemas**

1. **Organization of Data**:
   - Helps organize data into well-defined tables with relevant attributes.
   - Shows relationships between tables and specifies data types for each column.

2. **Efficiency**:
   - **Avoids Reverse Engineering**: Prevents the need for frequent reverse-engineering of the data model, saving time and effort.
   - **Efficient Queries**: Enables writing efficient queries for data retrieval, reporting, and analytics.

3. **Maintains Data Integrity**:
   - Ensures a clean set of data related to an application.
   - Reduces the risk of data inconsistencies and errors.

4. **Cost-Effective**:
   - Reduces time and effort spent on database design adjustments.
   - Lowers costs for organizations by minimizing the need for extensive redesigns.

In summary, a well-designed **database schema** is crucial for organizing data, maintaining data integrity, and ensuring efficient data retrieval. Understanding the three types of schemas—conceptual, internal, and external—helps in designing databases that meet various user needs while optimizing storage and access methods.

# Schema in use

### Creating a Simple Database Schema for a Shopping Cart

In this guide, you'll learn how to create a simple database schema using SQL. This schema will consist of three tables: `customer`, `product`, and `cart_order`. Follow the steps below to build the schema for a shopping cart database.

#### Step 1: Create the Database

First, create a new database called `shopping_cart_DB`.

```sql
CREATE DATABASE shopping_cart_DB;
```

After running this statement, the `shopping_cart_DB` database will appear in the left-hand Explorer.

#### Step 2: Create the `customer` Table

Next, create the `customer` table to store information about each customer. The table includes the following fields: `customer_id`, `name`, `address`, `email`, and `phone_number`.

```sql
USE shopping_cart_DB;

CREATE TABLE customer (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100),
    address VARCHAR(255),
    email VARCHAR(100),
    phone_number VARCHAR(10)
);
```

- `customer_id`: Integer, primary key
- `name`: Variable character, maximum 100 characters
- `address`: Variable character, maximum 255 characters
- `email`: Variable character, maximum 100 characters
- `phone_number`: Variable character, maximum 10 characters

#### Step 3: Create the `product` Table

Create the `product` table to store information about each product. The table includes the following fields: `product_id`, `name`, `price`, and `description`.

```sql
CREATE TABLE product (
    product_id INT PRIMARY KEY,
    name VARCHAR(100),
    price NUMERIC(8, 2),
    description VARCHAR(255)
);
```

- `product_id`: Integer, primary key
- `name`: Variable character, maximum 100 characters
- `price`: Numeric type with 8 digits and 2 decimal places
- `description`: Variable character, maximum 255 characters

#### Step 4: Create the `cart_order` Table

Create the `cart_order` table to store information about each order. The table includes the following fields: `order_id`, `customer_id`, `product_id`, `quantity`, `order_date`, and `status`.

```sql
CREATE TABLE cart_order (
    order_id INT PRIMARY KEY,
    customer_id INT,
    product_id INT,
    quantity INT,
    order_date DATE,
    status VARCHAR(100),
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id),
    FOREIGN KEY (product_id) REFERENCES product(product_id)
);
```

- `order_id`: Integer, primary key
- `customer_id`: Integer, foreign key referencing `customer(customer_id)`
- `product_id`: Integer, foreign key referencing `product(product_id)`
- `quantity`: Integer
- `order_date`: Date
- `status`: Variable character, maximum 100 characters

#### Explanation of Primary and Foreign Keys

- **Primary Key**: A unique identifier for a record in a table. For example, `customer_id` in the `customer` table, `product_id` in the `product` table, and `order_id` in the `cart_order` table.
- **Foreign Key**: A field in a table that links to the primary key of another table, establishing a relationship between the tables. In the `cart_order` table, `customer_id` and `product_id` are foreign keys that reference `customer(customer_id)` and `product(product_id)` respectively.

#### Running the SQL Statements

Execute the above SQL statements in your SQL environment to create the database and tables. Once executed, the `shopping_cart_DB` database will contain the `customer`, `product`, and `cart_order` tables, with the specified fields and relationships.

In this guide, you learned how to create a simple database schema using SQL. This process applies to both small and large-scale databases, providing a foundational understanding of organizing and managing data within a database.

# Types of database schema
### Understanding Different Types of Database Schemas

When developing databases, it is essential to distinguish between different kinds of database schemas to determine the best fit for your project. In this video, you'll explore logical and physical database schemas, understanding their definitions, purposes, and how they are used in practice.

#### Logical Database Schema

A logical database schema defines how data is organized in terms of tables and the relationships between these tables. It focuses on what tables should exist in a database and how the attributes of different tables are linked. This process is also known as Entity Relationship (ER) modeling.

**Key Concepts:**
- **Entities:** These are the objects or things in the database, such as orders, shipments, and couriers.
- **Attributes:** These are the properties or details of an entity, such as order ID, shipment ID, and courier ID.
- **Relationships:** These illustrate how entities are related to each other.

**Example of Logical Schema Using ER Modeling:**
Consider a simple ER model for an ordering application. The logical schema shows the relationships between orders, shipments, and couriers.

- **Order Entity:** Contains attributes such as `order_id` (primary key), `shipment_id` (foreign key), and `courier_id` (foreign key).
- **Shipment Entity:** Contains attributes such as `shipment_id` (primary key).
- **Courier Entity:** Contains attributes such as `courier_id` (primary key).

In this model:
- `order_id` uniquely identifies each order.
- `shipment_id` and `courier_id` are foreign keys in the `order` table, linking to the primary keys of the `shipment` and `courier` tables, respectively.

#### Physical Database Schema

A physical database schema defines how data is actually stored on disk. It involves creating the actual structure of the database using SQL statements or other database-specific code.

**Key Concepts:**
- **Tables:** Define the structure for storing data, including columns and data types.
- **Indexes:** Improve the speed of data retrieval.
- **Constraints:** Ensure data integrity and consistency, such as primary keys and foreign keys.
- **Storage Specifications:** Define how and where data is stored physically.

**Example of Physical Schema:**
For an online store database, you might create tables for customers, products, and transactions using SQL.

```sql
-- Creating the customer table
CREATE TABLE customer (
    customer_id INT PRIMARY KEY,
    name VARCHAR(100),
    address VARCHAR(255),
    email VARCHAR(100),
    phone_number VARCHAR(10)
);

-- Creating the product table
CREATE TABLE product (
    product_id INT PRIMARY KEY,
    name VARCHAR(100),
    price NUMERIC(8, 2),
    description VARCHAR(255)
);

-- Creating the transaction table
CREATE TABLE transaction (
    transaction_id INT PRIMARY KEY,
    customer_id INT,
    product_id INT,
    quantity INT,
    transaction_date DATE,
    status VARCHAR(100),
    FOREIGN KEY (customer_id) REFERENCES customer(customer_id),
    FOREIGN KEY (product_id) REFERENCES product(product_id)
);
```

In this example:
- The `customer`, `product`, and `transaction` tables are created with their respective columns and data types.
- Primary keys (`customer_id`, `product_id`, `transaction_id`) ensure each record is unique.
- Foreign keys (`customer_id`, `product_id` in `transaction` table) establish relationships between the tables.

#### Importance of Database Schemas

Database schemas are crucial in database creation as they form the foundation of your application. A well-designed schema ensures:
- **Data Organization:** Logical schema organizes data into well-defined tables with relevant attributes.
- **Data Relationships:** It defines relationships between tables, making data retrieval efficient.
- **Data Integrity:** Constraints and relationships ensure data integrity and consistency.
- **Efficient Queries:** Helps in writing efficient queries for data retrieval, reporting, and analytics.

#### Summary

- **Logical Database Schema:** Focuses on organizing data in terms of tables and relationships. It uses ER models to specify these relationships.
- **Physical Database Schema:** Defines how data is stored on disk using SQL statements. It includes the actual creation of tables, indexes, and constraints.

By understanding and utilizing both logical and physical schemas, you can effectively design and implement databases that meet the needs of your project.

# Building a schema

### Building a Database Schema for a Restaurant Booking System

In this reading, we will walk through the process of building a database schema for a restaurant booking scenario. The database schema will include tables for customers, waiters, tables in the restaurant, orders, reservations, menus, and menu items. Follow along and write the SQL code in a MySQL environment to see the schema you are building.

#### Logical Database Schema

The logical database schema for this scenario can be represented using an Entity-Relationship Diagram (ERD), which outlines the relationships between entities such as customers, tables, waiters, orders, reservations, menus, and menu items.

- **Customer:** Unique ID, name, NIC number, contact number.
- **Waiter:** Unique ID, name, contact number, shift.
- **Table:** Unique ID, location.
- **Order:** Unique ID, date_time, table_id, waiter_id.
- **Reservation:** Unique ID, date_time, number of guests, order_id, table_id, customer_id.
- **Menu:** Unique ID, description, availability.
- **Menu Item:** Unique ID, description, price, availability, menu_id.
- **Order Menu Item:** Composite key (order_id, menu_item_id), quantity.

#### Physical Database Schema

Now, let's translate the logical schema into SQL statements to create the physical schema for our restaurant booking system. 

1. **Create the Database:**
   ```sql
   CREATE DATABASE restaurant;
   USE restaurant;
   ```

2. **Create the Tables:**

   **Table for restaurant tables:**
   ```sql
   CREATE TABLE tbl (
       table_id INT PRIMARY KEY,
       location VARCHAR(255)
   );
   ```

   **Table for waiters:**
   ```sql
   CREATE TABLE waiter (
       waiter_id INT PRIMARY KEY,
       name VARCHAR(150),
       contact_no VARCHAR(10),
       shift VARCHAR(10)
   );
   ```

   **Table for orders:**
   ```sql
   CREATE TABLE table_order (
       order_id INT PRIMARY KEY,
       date_time DATETIME,
       table_id INT,
       waiter_id INT,
       FOREIGN KEY (table_id) REFERENCES tbl(table_id),
       FOREIGN KEY (waiter_id) REFERENCES waiter(waiter_id)
   );
   ```

   **Table for customers:**
   ```sql
   CREATE TABLE customer (
       customer_id INT PRIMARY KEY,
       name VARCHAR(100),
       NIC_no VARCHAR(12),
       contact_no VARCHAR(10)
   );
   ```

   **Table for reservations:**
   ```sql
   CREATE TABLE reservation (
       reservation_id INT PRIMARY KEY,
       date_time DATETIME,
       no_of_pax INT,
       order_id INT,
       table_id INT,
       customer_id INT,
       FOREIGN KEY (order_id) REFERENCES table_order(order_id),
       FOREIGN KEY (table_id) REFERENCES tbl(table_id),
       FOREIGN KEY (customer_id) REFERENCES customer(customer_id)
   );
   ```

   **Table for menus:**
   ```sql
   CREATE TABLE menu (
       menu_id INT PRIMARY KEY,
       description VARCHAR(255),
       availability INT
   );
   ```

   **Table for menu items:**
   ```sql
   CREATE TABLE menu_item (
       menu_item_id INT PRIMARY KEY,
       description VARCHAR(255),
       price FLOAT,
       availability INT,
       menu_id INT,
       FOREIGN KEY (menu_id) REFERENCES menu(menu_id)
   );
   ```

   **Table for ordered menu items:**
   ```sql
   CREATE TABLE order_menu_item (
       order_id INT,
       menu_item_id INT,
       quantity INT,
       PRIMARY KEY (order_id, menu_item_id),
       FOREIGN KEY (order_id) REFERENCES table_order(order_id),
       FOREIGN KEY (menu_item_id) REFERENCES menu_item(menu_item_id)
   );
   ```

#### Explanation of Relationships and Constraints

- **Primary Keys:** Ensure each record in a table is unique.
- **Foreign Keys:** Establish relationships between tables, ensuring referential integrity.
  - `table_order` references `tbl` and `waiter`.
  - `reservation` references `table_order`, `tbl`, and `customer`.
  - `menu_item` references `menu`.
  - `order_menu_item` references `table_order` and `menu_item`.


  ![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/kz9dpIFwT0WbW4GidJ97uA_762707d3c5ee4a20a790c06f7449d4a1_schema.png?expiry=1717891200000&hmac=4_dS9ZMAnv0AY7UhDl8myPALTKfCUp1LWxxsKKajOAU)

By following these steps, you create a robust database schema that captures the essential details and relationships needed for a restaurant booking system. This schema provides a solid foundation for further development and querying.

# Additional resources

- Prisma
https://www.prisma.io/dataguide/intro/intro-to-schemas

- Lucidchart
https://www.lucidchart.com/pages/database-diagram/database-schema

- Educative
https://www.educative.io/blog/what-are-database-schemas-examples

- IBM
https://www.ibm.com/topics/database-schema

