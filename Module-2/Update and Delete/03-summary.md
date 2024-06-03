# Updating data

Updating a database table is a common task, especially when managing dynamic data like student records. The SQL `UPDATE` statement allows you to modify existing records in your tables. Here's how you can use the `UPDATE` statement to change specific columns for one or more rows in your table.

### Updating a Single Record

To update the home address and contact number for a student with ID 3 in the `student` table, follow these steps:

1. **Open your SQL editor**: In phpMyAdmin, click on the SQL tab to open the SQL editor.
2. **Write the `UPDATE` statement**:

```sql
UPDATE student
SET home_address = '123 New Home Address', contact_number = '555-1234'
WHERE ID = 3;
```

- `UPDATE student`: Specifies the table to update.
- `SET home_address = '123 New Home Address', contact_number = '555-1234'`: Specifies the columns to update and their new values.
- `WHERE ID = 3`: Identifies which row(s) to update based on the ID.

3. **Execute the query**: Click the Go button to run the statement.
4. **Verify the update**: Check the table to confirm that the home address and contact number for the student with ID 3 have been updated.

### Updating Multiple Records

To update the college address for all engineering students to "Harper Building":

1. **Open your SQL editor**: In phpMyAdmin, click on the SQL tab to open the SQL editor.
2. **Write the `UPDATE` statement**:

```sql
UPDATE student
SET college_address = 'Harper Building'
WHERE department = 'Engineering';
```

- `UPDATE student`: Specifies the table to update.
- `SET college_address = 'Harper Building'`: Specifies the column to update and its new value.
- `WHERE department = 'Engineering'`: Identifies which rows to update based on the department.

3. **Execute the query**: Click the Go button to run the statement.
4. **Verify the update**: Check the table to confirm that the college address for all engineering students has been updated to "Harper Building".

### Updating Multiple Columns in Multiple Records

To update both the college address and home address for all engineering students:

1. **Open your SQL editor**: In phpMyAdmin, click on the SQL tab to open the SQL editor.
2. **Write the `UPDATE` statement**:

```sql
UPDATE student
SET college_address = 'Harper Building', home_address = 'New Home Address'
WHERE department = 'Engineering';
```

- `UPDATE student`: Specifies the table to update.
- `SET college_address = 'Harper Building', home_address = 'New Home Address'`: Specifies multiple columns to update and their new values.
- `WHERE department = 'Engineering'`: Identifies which rows to update based on the department.

3. **Execute the query**: Click the Go button to run the statement.
4. **Verify the update**: Check the table to confirm that both the college address and home address for all engineering students have been updated.

### Summary

Using the `UPDATE` statement, you can efficiently modify existing records in your database tables. Whether updating single or multiple columns and records, the key components of the `UPDATE` statement include the table name, the `SET` clause for specifying columns and new values, and the `WHERE` clause for identifying which records to update. By following these steps, you can ensure accurate and efficient updates to your database.

# Deleting data

Deleting records from a database table is a crucial operation that needs to be done with caution to avoid accidental data loss. The SQL `DELETE` statement allows you to remove specific rows or all rows from a table based on certain conditions. Here are the steps and examples for deleting records from a table using the `DELETE` statement in SQL:

### Deleting a Single Record

To delete a single record where the student's last name is 'Miller' from the `student` table:

1. **Open your SQL editor**: In phpMyAdmin, click on the SQL tab to open the SQL editor.
2. **Write the `DELETE` statement**:

```sql
DELETE FROM student
WHERE last_name = 'Miller';
```

- `DELETE FROM student`: Specifies the table from which to delete records.
- `WHERE last_name = 'Miller'`: Specifies the condition to identify the row to delete.

3. **Execute the query**: Click the Go button to run the statement.
4. **Verify the deletion**: Check the table to confirm that the record with the last name 'Miller' has been removed.

### Deleting Multiple Records

To delete records for all students in the engineering department from the `student` table:

1. **Open your SQL editor**: In phpMyAdmin, click on the SQL tab to open the SQL editor.
2. **Write the `DELETE` statement**:

```sql
DELETE FROM student
WHERE department = 'Engineering';
```

- `DELETE FROM student`: Specifies the table from which to delete records.
- `WHERE department = 'Engineering'`: Specifies the condition to identify the rows to delete.

3. **Execute the query**: Click the Go button to run the statement.
4. **Verify the deletion**: Check the table to confirm that the records for students in the engineering department have been removed.

### Deleting All Records

To delete all records from the `student` table:

1. **Open your SQL editor**: In phpMyAdmin, click on the SQL tab to open the SQL editor.
2. **Write the `DELETE` statement**:

```sql
DELETE FROM student;
```

- `DELETE FROM student`: Specifies the table from which to delete all records.

3. **Execute the query**: Click the Go button to run the statement.
4. **Verify the deletion**: Check the table to confirm that all records have been removed and the table is now empty.

### Summary

When using the `DELETE` statement in SQL:
- **For single record deletion**: Use a `WHERE` clause to specify the condition that uniquely identifies the record.
- **For multiple records deletion**: Use a `WHERE` clause to specify the condition that matches multiple records.
- **For deleting all records**: Use the `DELETE FROM table_name` statement without a `WHERE` clause.

Always double-check the `WHERE` clause to avoid unintended deletions. It's a good practice to back up your data before performing delete operations, especially when dealing with multiple records or the entire table.

# Module summary: Create, Read, Update and Delete (CRUD) Operations

### Recap of CRUD Operations Module

**Introduction to SQL Data Types**
- **Numeric Data Types**: 
  - *Understanding*: Differentiating between integer and decimal data types.
  - *Usage*: Storing numerical values, both positive and negative.
- **String Data Types**:
  - *Identification*: Recognizing string data types and their uses.
  - *Differences*: CHAR vs. VARCHAR - understanding fixed-length vs. variable-length storage.
- **Default Values**:
  - *Constraints*: Using default constraints to set default values in a table.
  - *Application*: Setting default values using SQL syntax.

**Key Skills Developed**:
1. **Numeric Data Types**:
   - Storing and managing numerical data of various sizes.
   - Utilizing integer and decimal data types effectively.
2. **String Data Types**:
   - Storing text-based data.
   - Differentiating and using CHAR and VARCHAR for efficient data storage.
3. **Default Values**:
   - Applying default constraints in database tables.
   - Ensuring tables have meaningful default values for certain columns.

**Creating and Reading Data**
- **Database and Table Creation**:
  - Creating databases and tables using SQL.
  - Modifying table structures with ALTER TABLE.
  - Dropping databases and tables when necessary.
- **Inserting Data**:
  - Adding data to tables with the INSERT INTO statement.
- **Reading Data**:
  - Retrieving data using the SELECT statement.
  - Querying and extracting data from one or more tables into another table using INSERT INTO SELECT.

**Practical Exercises**:
- Creating and populating databases and tables.
- Managing data within tables using SELECT and INSERT INTO SELECT statements.

**Updating and Deleting Data**
- **Updating Data**:
  - Using the UPDATE statement to modify single and multiple field values.
- **Deleting Data**:
  - Removing records from a table using the DELETE statement.

**Practical Exercises**:
- Updating records in a table with new information.
- Deleting specific records and ensuring the correct data is removed.

### Key Takeaways
- **Create**: Establish and define databases and tables.
- **Read**: Query and extract data efficiently.
- **Update**: Modify existing data within the database.
- **Delete**: Remove data securely and precisely.
- **Data Types**: Choosing the correct data type for data integrity and efficiency.
- **SQL Statements**: Mastery of INSERT, SELECT, UPDATE, and DELETE statements for data manipulation.

### Final Note
You now have a solid understanding and practical skills in handling CRUD operations in SQL, alongside an in-depth knowledge of various SQL data types. You're well-equipped to create, manage, and manipulate data in databases, contributing significantly to your learning journey in database management and SQL proficiency. Keep up the great work!