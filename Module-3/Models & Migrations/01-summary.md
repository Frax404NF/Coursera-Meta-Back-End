# Models

### Introduction to Django Models

In this module, you explored the model component of the Model-View-Template (MVT) architectural pattern in Django. Models are crucial for interacting with databases and performing CRUD operations in a structured and efficient manner. Let's recap the key concepts and techniques covered.

#### Overview of Models

1. **Definition:** A model in Django is the single definitive source of information about your data. It represents the data you are storing and contains essential fields and behaviors.
2. **Class Representation:** Each model is a Python class that subclasses `django.db.models.Model`. Attributes of the model represent database fields.
3. **Automatic API Generation:** Django provides an automatically generated database access API to interact with the database using Python code, eliminating the need for custom SQL queries.

#### Structure of Models

- **Model Class:** Define a model as a subclass of `django.db.models.Model`.
- **Attributes:** Each attribute of the model represents a column in the corresponding database table.
- **Primary Key:** Django automatically adds a primary key field, but you can override it if necessary.

#### CRUD Operations with Models

##### Create

- **SQL Equivalent:** `INSERT INTO user (first_name, last_name) VALUES ('John', 'Doe');`
- **Django:** Create a new instance of the model and save it.
    ```python
    user = User(first_name='John', last_name='Doe')
    user.save()
    ```

##### Read

- **SQL Equivalent:** `SELECT * FROM user WHERE id = 1;`
- **Django:** Use the `get` method to retrieve a specific record.
    ```python
    user = User.objects.get(id=1)
    ```

##### Update

- **SQL Equivalent:** `UPDATE user SET last_name='Smith' WHERE id=1;`
- **Django:** Retrieve the object, modify it, and save the changes.
    ```python
    user = User.objects.get(id=1)
    user.last_name = 'Smith'
    user.save()
    ```

##### Delete

- **SQL Equivalent:** `DELETE FROM user WHERE id=1;`
- **Django:** Retrieve the object and call the `delete` method.
    ```python
    user = User.objects.get(id=1)
    user.delete()
    ```

#### Using Migrations

- **Concept:** Migrations are Django’s way of propagating changes you make to your models (adding a field, deleting a model, etc.) into your database schema.
- **Process:** 
  1. Make changes to your model.
  2. Create migrations using `python manage.py makemigrations`.
  3. Apply migrations using `python manage.py migrate`.

#### Example Scenario: User Table

1. **SQL Table Creation:**
    ```sql
    CREATE TABLE user (
        id INT PRIMARY KEY,
        first_name VARCHAR(100),
        last_name VARCHAR(100)
    );
    ```

2. **Django Model:**
    ```python
    from django.db import models

    class User(models.Model):
        first_name = models.CharField(max_length=100)
        last_name = models.CharField(max_length=100)
    ```

3. **Performing CRUD Operations:**
    - **Create:**
        ```python
        user = User(first_name='John', last_name='Doe')
        user.save()
        ```
    - **Read:**
        ```python
        user = User.objects.get(id=1)
        ```
    - **Update:**
        ```python
        user = User.objects.get(id=1)
        user.last_name = 'Smith'
        user.save()
        ```
    - **Delete:**
        ```python
        user = User.objects.get(id=1)
        user.delete()
        ```

#### Conclusion

By leveraging Django models, you can efficiently manage your data without writing raw SQL queries. Models streamline CRUD operations, ensure consistency, and promote the use of Django’s ORM (Object-Relational Mapping) to interact with your database. Understanding and utilizing models effectively is a fundamental skill for any Django developer, enabling you to build robust and scalable web applications.

# Model relationships

### Model Relationships in Django

In Django, defining relationships between models is crucial for structuring data efficiently and maintaining data integrity. Here are the different types of relationships you can define in Django models:

#### Primary Key and Foreign Key

- **Primary Key:** A unique field for each row in a table, typically used as the main identifier.
- **Foreign Key:** A field in one table that uniquely identifies a row in another table. It ensures a link between the data in two tables.

Example:
- **Products Table:** `ProductID` is the primary key.
- **Customers Table:** `ProductID` is a foreign key referring to the `Products` table.

In the principal model, you must provide the CollegeID field as the foreign key. The on_delete option specifies the behavior in case the associated object in the primary model is deleted. The values are:

**CASCADE:** deletes the object containing the ForeignKey

**PROTECT:** Prevent deletion of the referenced object.

**RESTRICT:** Prevent deletion of the referenced object by raising RestrictedError

### Types of Relationships

#### 1. One-to-One Relationship

A one-to-one relationship implies that each record in one model is linked to one and only one record in another model.

**Example: College and Principal**

```python
class College(models.Model):
    CollegeID = models.IntegerField(primary_key=True)
    name = models.CharField(max_length=50)
    strength = models.IntegerField()
    website = models.URLField()

class Principal(models.Model):
    CollegeID = models.OneToOneField(
        College,
        on_delete=models.CASCADE
    )
    Qualification = models.CharField(max_length=50)
    email = models.EmailField(max_length=50)
```

**SQL Equivalent:**

```sql
CREATE TABLE "myapp_college" (
    "CollegeID" integer NOT NULL PRIMARY KEY, 
    "name" varchar(50) NOT NULL, 
    "strength" integer NOT NULL, 
    "website" varchar(200) NOT NULL
);

CREATE TABLE "myapp_principal" (
    "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
    "Qualification" varchar(50) NOT NULL, 
    "email" varchar(50) NOT NULL, 
    "CollegeID_id" integer NOT NULL UNIQUE REFERENCES "myapp_college" ("CollegeID") DEFERRABLE INITIALLY DEFERRED
);
```

#### 2. One-to-Many Relationship

A one-to-many relationship implies that one record in a model is associated with multiple records in another model.

**Example: Subject and Teacher**

```python
class Subject(models.Model):
    Subjectcode = models.IntegerField(primary_key=True)
    name = models.CharField(max_length=30)
    credits = models.IntegerField()

class Teacher(models.Model):
    TeacherID = models.IntegerField(primary_key=True)
    subjectcode = models.ForeignKey(
        Subject,
        on_delete=models.CASCADE
    )
    Qualification = models.CharField(max_length=50)
    email = models.EmailField(max_length=50)
```

**SQL Equivalent:**

```sql
CREATE TABLE "myapp_subject" (
    "Subjectcode" integer NOT NULL PRIMARY KEY, 
    "name" varchar(30) NOT NULL, 
    "credits" integer NOT NULL
);

CREATE TABLE "myapp_teacher" (
    "TeacherID" integer NOT NULL PRIMARY KEY, 
    "Qualification" varchar(50) NOT NULL, 
    "email" varchar(50) NOT NULL, 
    "subjectcode_id" integer NOT NULL REFERENCES "myapp_subject" ("Subjectcode") DEFERRABLE INITIALLY DEFERRED
);

CREATE INDEX "myapp_teacher_subjectcode_id_bef86dea" ON "myapp_teacher" ("subjectcode_id");
```

#### 3. Many-to-Many Relationship

A many-to-many relationship implies that multiple records in one model can be associated with multiple records in another model.

**Example: Subject and Teacher (Revised for Many-to-Many)**

```python
class Teacher(models.Model):
    TeacherID = models.IntegerField(primary_key=True)
    Qualification = models.CharField(max_length=50)
    email = models.EmailField(max_length=50)

class Subject(models.Model):
    Subjectcode = models.IntegerField(primary_key=True)
    name = models.CharField(max_length=30)
    credits = models.IntegerField()
    teacher = models.ManyToManyField(Teacher)
```

**SQL Equivalent:**

```sql
CREATE TABLE "myapp_teacher" (
    "TeacherID" integer NOT NULL PRIMARY KEY, 
    "Qualification" varchar(50) NOT NULL, 
    "email" varchar(50) NOT NULL
);

CREATE TABLE "myapp_subject" (
    "Subjectcode" integer NOT NULL PRIMARY KEY, 
    "name" varchar(30) NOT NULL, 
    "credits" integer NOT NULL
);

CREATE TABLE "myapp_subject_teacher" (
    "id" integer NOT NULL PRIMARY KEY AUTOINCREMENT, 
    "subject_id" integer NOT NULL REFERENCES "myapp_subject" ("Subjectcode") DEFERRABLE INITIALLY DEFERRED, 
    "teacher_id" integer NOT NULL REFERENCES "myapp_teacher" ("TeacherID") DEFERRABLE INITIALLY DEFERRED
);

CREATE UNIQUE INDEX "myapp_subject_teacher_subject_id_teacher_id_9b6a3c00_uniq" ON "myapp_subject_teacher" ("subject_id", "teacher_id");
CREATE INDEX "myapp_subject_teacher_subject_id_e87c76e7" ON "myapp_subject_teacher" ("subject_id");
CREATE INDEX "myapp_subject_teacher_teacher_id_359f8cce" ON "myapp_subject_teacher" ("teacher_id");
```

### Conclusion

Understanding and implementing the appropriate relationships between models is key to maintaining data integrity and avoiding redundancy in your database. Django makes it straightforward to define these relationships using model fields such as `OneToOneField`, `ForeignKey`, and `ManyToManyField`. By leveraging these relationships, you can build a well-structured and efficient database schema that reflects the real-world relationships between different entities in your application.

# Creating models

### Creating and Managing Models in Django

In this guide, you'll learn how to create and manage models in Django, a critical aspect of building dynamic web applications. Models represent the structure of your database tables and allow you to interact with the database using Python code.

#### Step-by-Step Guide

1. **Setup Django Project and App**
   - Create a Django project (e.g., `menu_project`).
   - Create a Django app (e.g., `menuapp`).

2. **Define a Model in `models.py`**
   - Open `models.py` in your app directory.
   - Define a model for a restaurant menu.

```python
# menuapp/models.py

from django.db import models

class Menu(models.Model):
    name = models.CharField(max_length=100)
    cuisine = models.CharField(max_length=100)
    price = models.IntegerField()

    def __str__(self):
        return self.name
```

3. **Register the App in `settings.py`**
   - Add your app to the `INSTALLED_APPS` list in `settings.py`.

```python
# menu_project/settings.py

INSTALLED_APPS = [
    # other installed apps
    'menuapp',
]
```

4. **Create and Apply Migrations**
   - Open the terminal and run the following commands to create and apply migrations:

```bash
python manage.py makemigrations
python manage.py migrate
```

These commands will generate and apply the SQL commands needed to create the corresponding database table.

5. **Interact with the Model via Django Shell**
   - Open the Django shell to interact with your model.

```bash
python manage.py shell
```

   - Import and use your model in the shell.

```python
from menuapp.models import Menu

# Create a new menu item
menu_item = Menu.objects.create(name='Pasta', cuisine='Italian', price=10)
menu_item.save()

# Retrieve all menu items
menu_items = Menu.objects.all()
print(menu_items)
```

6. **Update and Delete Entries**
   - To update an entry, retrieve it and then modify its attributes.

```python
# Retrieve the menu item with primary key 1
menu_item = Menu.objects.get(id=1)
menu_item.cuisine = 'Mexican'
menu_item.save()
```

   - To delete an entry:

```python
menu_item = Menu.objects.get(id=1)
menu_item.delete()
```

### Additional Features

1. **Customizing the Model String Representation**
   - Override the `__str__` method in your model to provide a meaningful string representation.

```python
class Menu(models.Model):
    # fields...

    def __str__(self):
        return f"{self.name} ({self.cuisine}) - ${self.price}"
```

2. **Using Django Admin Interface**
   - Enable Django's admin interface for easy data management.

```python
# menuapp/admin.py

from django.contrib import admin
from .models import Menu

admin.site.register(Menu)
```

   - Create a superuser to access the admin interface.

```bash
python manage.py createsuperuser
```

   - Access the admin interface at `http://127.0.0.1:8000/admin/` and manage your menu items through a web interface.

### Conclusion

By following these steps, you've created a Django model, performed basic CRUD operations, and explored additional features like custom string representations and the Django admin interface. This foundational knowledge is essential for developing more complex applications using Django.

# Migrations

### Understanding Migrations in Django

In web application development, application requirements often change, necessitating modifications to the data model. Django uses a system called migrations to handle these changes efficiently. Here’s an overview of how migrations work and why they are beneficial:

#### Models and ORM

Django is designed to work with relational databases like PostgreSQL, MySQL, and SQLite, where data is organized in tables. Models in Django represent these data tables. The models define database fields, which correspond to columns in their associated database tables. For instance, a model `User` would represent a `User` table, and attributes of the model, such as `first_name`, correspond to columns in the table.

Django's Object-Relational Mapper (ORM) allows developers to manipulate database records using Python code instead of writing SQL queries. This means you can add, update, and delete data directly through the code.

#### Changing the Database Schema

As applications evolve, changes to the database schema are often necessary. This can include adding new attributes (columns), renaming columns, or deleting models. Django handles these operations using migrations, which are records of changes made to models. Migrations are stored as files in a `migrations` folder within each app.

#### Example: Adding a Column

Consider adding a new column `City` to the `User` table:
1. **Without ORM:** You would write an SQL `ALTER` statement to add the column.
2. **With Django:** Simply add the new attribute to the `User` model in your code.

#### Creating and Applying Migrations

1. **Create Migration Script:** After updating the model, you create a migration script. This script contains instructions on how to alter the database schema to reflect changes in the models.
   - Command: `python manage.py makemigrations`
2. **Apply Migrations:** Apply the migration script to update the database schema.
   - Command: `python manage.py migrate`

#### Benefits of Migrations

1. **Syncing Issues:** Migrations help avoid syncing issues between the models and the database. Each developer can update their local database by running migration scripts, ensuring consistency.
2. **Version Control:** Migrations are kept in version control, providing a history of changes and allowing developers to see who made what changes.
3. **Ease of Maintenance:** Managing database changes through code makes maintenance easier. Developers don’t need to write SQL queries or manage separate SQL script files.

### Steps to Add a New Column Using Migrations

1. **Update the Model:**
   ```python
   class User(models.Model):
       first_name = models.CharField(max_length=100)
       last_name = models.CharField(max_length=100)
       city = models.CharField(max_length=100)  # New attribute
   ```

2. **Create Migration Script:**
   ```bash
   python manage.py makemigrations
   ```

3. **Apply Migrations:**
   ```bash
   python manage.py migrate
   ```

### Conclusion

Migrations in Django provide a systematic way to handle changes in the database schema. They integrate with version control, minimize syncing issues, and simplify database maintenance. By managing database changes through code, developers can ensure a consistent and efficient development process.

# How to use migrations

### Understanding Django Migrations

Django migrations are a powerful mechanism for translating Django models into database tables and managing changes over time. Here's a detailed and structured summary of how to use migrations in Django:

#### Key Commands in Django Migrations

1. **makemigrations**: Detects changes in models and creates new migration scripts.
2. **migrate**: Applies the migration scripts to the database, making actual changes.
3. **sqlmigrate**: Displays the SQL statements for a given migration.
4. **showmigrations**: Lists all migrations and their current status (applied or pending).

#### Basic Workflow of Using Migrations

1. **Creating Initial Migrations**:
    - When you add a new model or change an existing one, run the `makemigrations` command.
    - This creates a migration script that records the changes.

    ```bash
    python manage.py makemigrations
    ```

2. **Applying Migrations**:
    - To implement the changes in the database, run the `migrate` command.
    
    ```bash
    python manage.py migrate
    ```

3. **Viewing SQL Queries**:
    - Use the `sqlmigrate` command to see the SQL queries that will be executed.

    ```bash
    python manage.py sqlmigrate <app_name> <migration_name>
    ```

4. **Listing Migrations**:
    - Use the `showmigrations` command to see the status of migrations.

    ```bash
    python manage.py showmigrations
    ```

#### Migrating Models of INSTALLED_APPS

When a Django project is created, certain apps are installed by default. These apps are listed in the `INSTALLED_APPS` section of the `settings.py` file. Running the `migrate` command applies migrations for these apps, creating the necessary tables in the database.

Example:
```bash
python manage.py migrate
```

#### Creating and Applying Migrations for a New App

1. **Creating a New App**:
    - Create a new app using the `startapp` command.

    ```bash
    python manage.py startapp myapp
    ```

2. **Defining Models**:
    - Define models in the `models.py` file of your app.

    ```python
    from django.db import models

    class Person(models.Model):
        name = models.CharField(max_length=20)
        email = models.EmailField()
        phone = models.CharField(max_length=20)
    ```

3. **Making Migrations**:
    - Create migration scripts for your models.

    ```bash
    python manage.py makemigrations
    ```

4. **Applying Migrations**:
    - Apply the migration scripts to the database.

    ```bash
    python manage.py migrate
    ```

#### Modifying Models and Managing Migrations

1. **Updating Models**:
    - Modify your model class and run `makemigrations` to create new migration scripts.
    - Example: Renaming a field and adding a new field.

    ```python
    class Person(models.Model):
        person_name = models.CharField(max_length=20)
        email = models.EmailField()
        phone = models.CharField(max_length=20)
        age = models.IntegerField(null=True)
    ```

    ```bash
    python manage.py makemigrations
    ```

2. **Checking Migration Status**:
    - Use the `showmigrations` command to see which migrations have been applied.

    ```bash
    python manage.py showmigrations
    ```

3. **Applying Pending Migrations**:
    - Run the `migrate` command to apply pending migrations.

    ```bash
    python manage.py migrate
    ```

#### Version Control with Migrations

- **Rolling Back Migrations**:
  - You can revert to a specific migration by running the `migrate` command with the target migration name.

  ```bash
  python manage.py migrate <app_name> <migration_name>
  ```

#### Using sqlmigrate for Debugging

- The `sqlmigrate` command shows the SQL that Django will run for a specific migration. This is useful for debugging.

  ```bash
  python manage.py sqlmigrate myapp 0001_initial
  ```

### Summary

Django's migration system is an efficient version control system for database schemas, enabling developers to manage changes in their models effectively. The core commands (`makemigrations`, `migrate`, `sqlmigrate`, and `showmigrations`) facilitate the creation, application, and management of database migrations, ensuring that the database schema evolves in sync with the models.

# Working with Migrations

### How to Apply Migrations in a Django Project

In this guide, we will learn how to apply migrations in a Django project using key commands like `makemigrations`, `migrate`, and `sqlmigrate`. We'll also explore how migrations provide version control for models and how to review generated SQL commands. Let's walk through the steps with a practical example.

#### Step-by-Step Process

1. **Project and App Setup**:
    - Ensure you have a Django project with an app called `myapp`.
    - Inside `myapp/models.py`, define a model class `Menuitems` with attributes `name`, `course`, and `year`.

2. **Update INSTALLED_APPS**:
    - Open `settings.py` in your project folder.
    - Add `'myapp'` to the `INSTALLED_APPS` list to include your app in the project.

    ```python
    INSTALLED_APPS = [
        ...
        'myapp',
    ]
    ```

3. **Create Initial Migration**:
    - Run the `makemigrations` command to prepare the changes.

    ```bash
    python manage.py makemigrations
    ```

    - Django will generate a migration file (e.g., `0001_initial.py`) for creating the `Menuitems` model.

4. **Apply the Migration**:
    - Run the `migrate` command to apply the migration and create the database table.

    ```bash
    python manage.py migrate
    ```

#### Modifying the Model

1. **Change an Attribute**:
    - Modify the `Menuitems` model by changing `course` to `category` in `models.py`.

    ```python
    class Menuitems(models.Model):
        item_name = models.CharField(max_length=100)
        category = models.CharField(max_length=50)
        year = models.IntegerField()
    ```

2. **Run makemigrations Again**:
    - Run `makemigrations` to detect the changes.

    ```bash
    python manage.py makemigrations
    ```

    - Confirm the change when prompted (e.g., renaming `course` to `category`).

3. **Apply the Latest Migration**:
    - Apply the migration with the `migrate` command.

    ```bash
    python manage.py migrate
    ```

4. **Check Migrations Status**:
    - Use `showmigrations` to list all migrations and their status.

    ```bash
    python manage.py showmigrations
    ```

#### Reverting Migrations

- To revert to a previous migration, specify the migration name.

    ```bash
    python manage.py migrate myapp 0001_initial
    ```

    - Optionally, use the `--plan` flag to preview changes.

    ```bash
    python manage.py migrate myapp 0001_initial --plan
    ```

#### Viewing SQL Commands

- Use `sqlmigrate` to see the SQL commands generated by a migration.

    ```bash
    python manage.py sqlmigrate myapp 0001_initial
    ```

    - This command shows the SQL `CREATE TABLE` statements or other SQL commands for the specified migration.

### Example Workflow

1. **Initial Setup and Migration**:
    ```python
    # models.py
    class Menuitems(models.Model):
        name = models.CharField(max_length=100)
        course = models.CharField(max_length=50)
        year = models.IntegerField()
    ```

    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

2. **First Modification**:
    ```python
    # models.py
    class Menuitems(models.Model):
        name = models.CharField(max_length=100)
        category = models.CharField(max_length=50)
        year = models.IntegerField()
    ```

    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

3. **Second Modification**:
    ```python
    # models.py
    class Menuitems(models.Model):
        item_name = models.CharField(max_length=100)
        category = models.CharField(max_length=50)
        year = models.IntegerField()
    ```

    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

4. **Review and Revert**:
    ```bash
    python manage.py showmigrations
    python manage.py migrate myapp 0001_initial
    python manage.py sqlmigrate myapp 0001_initial
    ```

### Conclusion

Migrations in Django are essential for managing changes to your database schema over time. By using commands like `makemigrations`, `migrate`, and `sqlmigrate`, developers can efficiently apply and track changes, ensuring a robust version control system for their database models.

# A history of changes

### Understanding Django Migrations: History and Version Control

Django migrations play a crucial role in translating Django models into database tables and managing changes over time. They provide a systematic approach to apply version control to the model code base, making it easier to track and manage database schema changes.

#### Advantages of Using Migrations

1. **Direct Model Querying**:
    - Developers can query data directly from the code using models instead of writing raw SQL commands.
    - This approach leverages Django's ORM (Object-Relational Mapping) to simplify database interactions.

2. **Change History and Version Control**:
    - Migrations maintain a history of changes made to the models, providing a clear record of how each model was created and modified over time.
    - This history is essential for understanding the evolution of the database schema.

3. **Incremental Changes**:
    - Web application development often involves small, incremental changes. Migrations handle these changes efficiently, such as adding or renaming model attributes.

4. **Multi-Database Synchronization**:
    - Migrations ensure that schema changes are consistently applied across multiple databases if used, keeping them synchronized.

5. **Avoiding Repetition**:
    - Migrations automate the creation of database schemas from models, eliminating the need for repetitive SQL queries.
    - This aligns with Django's principle of "Don't Repeat Yourself" (DRY).

#### How Django Keeps Track of Migrations

1. **Migration Files**:
    - Migration files are stored in the `migrations` folder within each app.
    - These files are automatically updated after each migration, reflecting the latest schema changes.

2. **Migration File Structure**:
    - Each migration file is named with an auto-incremental prefix (e.g., `0001_initial.py`), which indicates the order of migrations.
    - The filenames are generated based on the actions performed or timestamps.

3. **Tracking Applied Migrations**:
    - Django creates a table called `django_migrations` in the database to keep track of applied migrations.
    - This table includes columns for ID (auto-incremented), app name, migration name, and timestamp.

4. **Dependencies and Operations**:
    - Each migration file contains a `Migration` class with `dependencies` and `operations`.
    - **Dependencies** specify the migrations that must be applied before this one.
    - **Operations** define the actions performed in the migration, such as creating or altering tables.

    ```python
    from django.db import migrations, models

    class Migration(migrations.Migration):
        dependencies = [
            ('myapp', '0001_initial'),
        ]

        operations = [
            migrations.CreateModel(
                name='Menuitems',
                fields=[
                    ('id', models.AutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                    ('name', models.CharField(max_length=100)),
                    ('course', models.CharField(max_length=50)),
                    ('year', models.IntegerField()),
                ],
            ),
        ]
    ```

#### Common Migration Operations

1. **CreateModel**: Creates a new model and corresponding database table.
2. **DeleteModel**: Deletes a model and drops the corresponding table.
3. **AddField**: Adds a new field (column) to an existing model.
4. **AlterField**: Alters an existing field (column) in a model.
5. **AddIndex**: Adds an index to a field in a model.

Example of `DeleteModel` operation:
```python
operations = [
    migrations.DeleteModel(
        name='Menuitems',
    ),
]
```

This operation will generate an SQL query like:
```sql
DROP TABLE menuitems;
```

#### Applying Migrations in Django

1. **Creating Initial Migrations**:
    - Define your models in `models.py`.

    ```python
    class Menuitems(models.Model):
        name = models.CharField(max_length=100)
        course = models.CharField(max_length=50)
        year = models.IntegerField()
    ```

    - Run `makemigrations` to create the initial migration script.

    ```bash
    python manage.py makemigrations
    ```

    - Run `migrate` to apply the migration and create the database table.

    ```bash
    python manage.py migrate
    ```

2. **Modifying Models**:
    - Update your models (e.g., rename `course` to `category`).

    ```python
    class Menuitems(models.Model):
        item_name = models.CharField(max_length=100)
        category = models.CharField(max_length=50)
        year = models.IntegerField()
    ```

    - Run `makemigrations` and `migrate` to apply the changes.

    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

3. **Viewing Migration History**:
    - Use `showmigrations` to list all migrations and their statuses.

    ```bash
    python manage.py showmigrations
    ```

4. **Reverting Migrations**:
    - Revert to a previous migration if needed.

    ```bash
    python manage.py migrate myapp 0001_initial
    ```

5. **Viewing SQL Commands**:
    - Use `sqlmigrate` to see the SQL commands generated by a migration.

    ```bash
    python manage.py sqlmigrate myapp 0001_initial
    ```

### Conclusion

Django migrations provide an efficient and systematic way to manage database schema changes. By using commands like `makemigrations`, `migrate`, and `sqlmigrate`, developers can apply version control to their models, ensuring consistency and reducing repetitive tasks. Migrations help track the history of changes, making it easier to manage and evolve the database schema in line with the application's development needs.

# Exercise: Models and migrations
Objectives
- The learner will create a model called Drinks
- The learner will perform the steps required to apply migrations

# Models using Foreign Keys

In this guide, we will delve into creating and managing one-to-many relationships in Django using foreign keys. Specifically, we will build an example model for an online menu at the Little Lemon restaurant, demonstrating how to categorize menu items using foreign keys.

#### Step-by-Step Process

1. **Creating Models with Foreign Keys**:
    - Start by defining two models in `models.py`: one for `MenuCategory` and another for `Menu`.
    - The `Menu` model will include a foreign key to `MenuCategory` to establish a one-to-many relationship.

2. **Defining the Models**:
    - Open the `models.py` file in your Django app and define the `MenuCategory` and `Menu` models.

    ```python
    from django.db import models

    class MenuCategory(models.Model):
        name = models.CharField(max_length=100)

        def __str__(self):
            return self.name

    class Menu(models.Model):
        name = models.CharField(max_length=100)
        price = models.IntegerField()
        category = models.ForeignKey(MenuCategory, on_delete=models.PROTECT, default=None, related_name='menu_items')

        def __str__(self):
            return self.name
    ```

3. **ForeignKey Parameters**:
    - **First Parameter**: Specifies the model to which the foreign key refers.
    - **on_delete**: Defines the behavior when the referenced object is deleted. Using `models.PROTECT` ensures that deletion of a category will not cascade to the `Menu` items.
    - **related_name**: Allows you to refer to the related objects from the `MenuCategory` side. For example, you can access all menu items of a category with `category.menu_items.all()`.

4. **Registering Models in Admin**:
    - Update `admin.py` to register your models so you can manage them through Django’s admin interface.

    ```python
    from django.contrib import admin
    from .models import MenuCategory, Menu

    admin.site.register(MenuCategory)
    admin.site.register(Menu)
    ```

5. **Running Migrations**:
    - Ensure your `INSTALLED_APPS` in `settings.py` includes your app.
    - Run the following commands to create and apply migrations:

    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

6. **Inspecting the Database**:
    - Use SQLite Explorer in your IDE or a database management tool to inspect the generated tables.
    - The `Menu` table will include a `category_id` column linking to the `MenuCategory` table.

#### Example Database Inspection

- **MenuCategory Table**:
    - Contains rows like `ID: 1, Name: Italian`, `ID: 2, Name: Greek`, `ID: 3, Name: Turkish`.

- **Menu Table**:
    - Contains rows where the `category_id` column references the `MenuCategory` table, such as `ID: 1, Name: Greek Salad, Category_id: 2`.

#### Enhancing Readability

To make the foreign key relationship more intuitive in queries and templates, we use the `related_name` attribute:

- **Accessing Related Items**:
    - With `related_name='menu_items'`, you can access all menu items for a category via `category.menu_items.all()`.

#### Protecting Deletions

- **models.PROTECT**:
    - Ensures that a category cannot be deleted if it is still referenced by any menu items, preserving database integrity.

### Summary

In this guide, you learned how to create and manage one-to-many relationships in Django using foreign keys. By defining models, setting up foreign keys, and managing migrations, you can efficiently organize and relate data in your Django applications. This approach simplifies data management and ensures consistency across your database.

#### Key Commands

- **Creating Migrations**:
    ```bash
    python manage.py makemigrations
    ```

- **Applying Migrations**:
    ```bash
    python manage.py migrate
    ```

- **Inspecting Migrations**:
    ```bash
    python manage.py showmigrations
    ```

- **Viewing SQL Queries**:
    ```bash
    python manage.py sqlmigrate myapp 0001
    ```

By following these steps, you can leverage Django's powerful ORM to manage complex database relationships effectively.

# Object Relationship Mapping - ORM

### Understanding Object-Relational Mapping (ORM) in Django

#### What is ORM?

Object-Relational Mapping (ORM) is a technique used in programming languages like Python to convert data between incompatible systems using object-oriented programming (OOP) principles. ORMs allow developers to interact with databases using the programming language's syntax and structures, rather than writing raw SQL queries. This simplifies database interactions and allows for more intuitive coding practices.

In Django, the ORM layer maps Python classes to database tables. Each attribute of the model represents a field in the table, enabling developers to perform database operations in an object-oriented way. This abstraction allows developers to focus on business logic rather than the intricacies of database interactions.

#### Key Features of Django's ORM

1. **Model Definition**:
   - Each model is a Python class that subclasses `django.db.models.Model`.
   - Attributes of the model correspond to database fields.

2. **CRUD Operations**:
   - Models provide a straightforward way to perform Create, Read, Update, and Delete (CRUD) operations.
   - Django's ORM supports these operations via methods on model instances and manager classes.

3. **Migration Mechanism**:
   - Django’s migration system propagates changes in models to the database schema, ensuring the database structure aligns with the models.

4. **QuerySets**:
   - A QuerySet is a collection of database queries representing a set of rows from the database.
   - QuerySets allow for filtering, ordering, and retrieving data from the database.

#### Example: Creating and Managing Models in Django

Let's walk through a practical example to understand how ORM works in Django.

1. **Define Models**:
    - Create a Django app called `demoapp`.
    - Define two models, `Customer` and `Vehicle`, with a one-to-many relationship.

    ```python
    from django.db import models  

    class Customer(models.Model): 
        name = models.CharField(max_length=255)

        def __str__(self):
            return self.name

    class Vehicle(models.Model): 
        name = models.CharField(max_length=255) 
        customer = models.ForeignKey(Customer, on_delete=models.CASCADE, related_name='vehicles')

        def __str__(self):
            return self.name
    ```

2. **Run Migrations**:
    - Apply migrations to create the corresponding tables in the database.

    ```bash
    python manage.py makemigrations
    python manage.py migrate
    ```

3. **Using the Django Shell**:
    - Open the Django shell to interact with the models.

    ```bash
    python manage.py shell
    ```

4. **Create and Save Objects**:
    - Create and save `Customer` objects.

    ```python
    from demoapp.models import Customer, Vehicle

    c1 = Customer(name="Henry")
    c1.save()

    c2 = Customer.objects.create(name="Hameed")
    ```

    - Create and save `Vehicle` objects.

    ```python
    v1 = Vehicle(name="Honda", customer=c2)
    v1.save()

    v2 = Vehicle.objects.create(name="Toyota", customer=c2)

    c1 = Customer.objects.get(name="Henry")
    Vehicle.objects.create(name="Ford", customer=c1)
    Vehicle.objects.create(name="Nissan", customer=c1)
    ```

5. **Fetch Objects with QuerySets**:
    - Retrieve all `Customer` objects.

    ```python
    customers = Customer.objects.all()
    [customer.name for customer in customers]  # Output: ['Henry', 'Hameed']
    ```

    - Retrieve all `Vehicle` objects and their associated `Customer` names.

    ```python
    vehicles = Vehicle.objects.all()
    for vehicle in vehicles:
        print(vehicle.name, ":", vehicle.customer.name)
    # Output:
    # Honda : Hameed
    # Toyota : Hameed
    # Ford : Henry
    # Nissan : Henry
    ```

6. **Filter QuerySets**:
    - Retrieve customers whose names start with 'H'.

    ```python
    customers_with_h = Customer.objects.filter(name__startswith='H')
    ```

7. **Update and Delete Objects**:
    - Update a customer's name.

    ```python
    c = Customer.objects.get(name="Henry")
    c.name = "Helen"
    c.save()
    ```

    - Delete a customer.

    ```python
    c = Customer.objects.get(pk=4)
    c.delete()
    ```

#### Benefits of Using Django's ORM

- **Simplifies Database Interactions**: Enables developers to interact with the database using Python code rather than raw SQL.
- **Promotes DRY Principle**: Reduces redundancy by abstracting repetitive SQL queries.
- **Ensures Consistency**: Migration system ensures the database schema matches the models.
- **Enhances Productivity**: Speeds up development by allowing developers to focus on application logic.

Django's ORM is a powerful tool that simplifies database interactions, ensures data consistency, and enhances developer productivity by providing an intuitive and efficient way to manage database operations through Python code.

# Using ORM

### Understanding Object-Relational Mapping (ORM) in Django

#### What is ORM?

Object-Relational Mapping (ORM) is a technique that allows developers to interact with a database using the object-oriented paradigm of their programming language, rather than writing raw SQL queries. ORM provides an abstraction layer that maps Python objects to database tables, enabling developers to perform database operations using Python code.

#### How Django Uses ORM

Django's ORM facilitates interaction between the Django application and the database by automatically generating the necessary SQL queries. This abstraction allows developers to focus on writing Python code without worrying about the underlying SQL.

#### QuerySet API

A QuerySet is a collection of database queries to fetch and manipulate objects from the database. In Django, the QuerySet API provides a way to construct, filter, and retrieve database records using Python code.

#### Using Django's ORM: A Practical Example

Let's explore how Django's ORM works with an example. Assume we have a model called `Customer` with attributes such as `name`, `reservation_day`, and `num_of_seats`. This model has already been migrated, and the database contains some entries.

##### Define the Models

In the `models.py` file, define the `Customer` model:

```python
from django.db import models  

class Customer(models.Model): 
    name = models.CharField(max_length=255)
    reservation_day = models.CharField(max_length=50)
    num_of_seats = models.IntegerField()

    def __str__(self):
        return self.name
```

##### Run Migrations

Apply migrations to create the corresponding tables in the database:

```bash
python manage.py makemigrations
python manage.py migrate
```

##### Using the Django Shell

Open the Django shell to interact with the models:

```bash
python manage.py shell
```

##### Create and Save Objects

Create and save `Customer` objects:

```python
from myapp.models import Customer

# Creating and saving a customer
c1 = Customer(name="James", reservation_day="Saturday", num_of_seats=4)
c1.save()

# Using the create() method to add another customer
c2 = Customer.objects.create(name="Jacqueline", reservation_day="Saturday", num_of_seats=2)
```

##### Fetch Objects with QuerySets

Retrieve all `Customer` objects using a QuerySet:

```python
customers = Customer.objects.all()
for customer in customers:
    print(customer.name)
```

##### Filter QuerySets

Use the `filter` method to retrieve specific entries. For example, get all customers with reservations on Saturday:

```python
saturday_customers = Customer.objects.filter(reservation_day="Saturday")
for customer in saturday_customers:
    print(customer.name)
```

You can add more conditions to the filter to narrow down the results. For example, find customers with reservations on Friday who requested 4 seats:

```python
friday_customers = Customer.objects.filter(reservation_day="Friday", num_of_seats=4)
for customer in friday_customers:
    print(customer.name)
```

##### Retrieve a Single Object

Use the `get` method to retrieve a single object by its primary key (ID):

```python
customer = Customer.objects.get(id=1)
print(customer.name)
```

##### Update an Object

To update an object, fetch it, modify its attributes, and save it:

```python
customer = Customer.objects.get(name="James")
customer.name = "Jim"
customer.save()
```

##### Delete an Object

To delete an object, fetch it and call the `delete` method:

```python
customer = Customer.objects.get(name="Jacqueline")
customer.delete()
```

#### Benefits of Using Django's ORM

- **Simplifies Database Interactions**: Allows developers to interact with the database using Python objects rather than writing raw SQL queries.
- **Promotes DRY Principle**: Reduces redundancy by abstracting repetitive SQL queries.
- **Ensures Consistency**: The migration system ensures that the database schema matches the models.
- **Enhances Productivity**: Speeds up development by allowing developers to focus on application logic.

Django's ORM is a powerful tool that streamlines database interactions, ensures data consistency, and enhances developer productivity by providing an intuitive and efficient way to manage database operations through Python code.

# Additional resources
The following resources will be helpful you learn more about different concepts related to Models and Migrations.

Models – official documentation
- https://docs.djangoproject.com/en/4.1/topics/db/models/

Migrations – official documentation
- https://docs.djangoproject.com/en/4.1/topics/migrations/

Other Model Instance methods (including the String Dunder method)
- https://docs.djangoproject.com/en/dev/ref/models/instances/#model-instance-methods

Using Models – Mozilla
- https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Models

Detailed overview of Migrations
- https://docs.djangoproject.com/en/4.1/topics/migrations/

Migration operations
- https://docs.djangoproject.com/en/4.1/ref/migration-operations/

Understanding ORM
- https://docs.djangoproject.com/en/4.1/ref/models/class/