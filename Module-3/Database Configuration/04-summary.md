# Database options

### Summary of Configuring Databases in a Django Project

When creating a Django project, the default configuration uses an SQLite database. This is set automatically in the `settings.py` file, allowing immediate work with the database. SQLite is a user-friendly, zero-configuration database with several advantages, especially for small projects or rapid prototyping. It doesn't require installation or running as a server process, meaning you don't need to start or stop it or use additional configuration files.

### Advantages of SQLite
- **User-Friendly**: No additional setup or configuration needed.
- **Zero Configuration**: Works out of the box without running as a server.
- **Ideal for Small Projects**: Great for rapid development and testing environments.

### Transitioning to a More Robust Database

For larger or production environments, a more scalable database like MySQL might be needed. Django supports multiple databases with minimal configuration, including PostgreSQL, MariaDB, MySQL, Oracle, and SQLite. MySQL and PostgreSQL are the most commonly used.

### Configuring MySQL in Django

#### Steps to Connect to MySQL:
1. **Database Details**: Provide the database's address, port number, and name.
2. **Database Driver**: Install the MySQL client which translates Python queries into SQL.
3. **Settings Configuration**: 
   - Configure connection parameters in `settings.py` under the `DATABASES` option.
   - The default is for SQLite; change this to MySQL settings.
   - Use an options file to store database credentials like name, user, and password, which is usually located in `etc/mysql` for security reasons.

#### Connection Management:
- **CONN_MAX_AGE Parameter**: Controls how long a connection remains open before being closed.

#### Database Creation:
- **Manual Setup**: Manually create the database initially.
- **Migration Scripts**: Django creates tables based on your models from migration scripts.

### Security Considerations

- **Role Assignment**: Assign security roles and use strong usernames and passwords.
- **Credential Security**: Protect database credentials to prevent unauthorized access.
- **Data Privacy**: Ensure personal information stored in the database is kept secure.

By following these steps and security measures, you can effectively configure and manage a MySQL database within a Django project.

# Configuring MySQL Connection

### Configuring MySQL Connection in a Django Project

#### Introduction
By default, Django uses the SQLite database, which is built-in and requires no setup. However, Django also supports other databases like PostgreSQL, MariaDB, MySQL, and Oracle. This guide explains how to configure MySQL with a Django application.

### Advantages of MySQL
While SQLite is useful for small applications and prototypes due to its simplicity and serverless nature, it has limitations, including lack of scalability and user management. MySQL, on the other hand, offers:
- **Scalability**: Suitable for larger applications.
- **Authentication**: Robust user management system.

### Installing MySQL
1. **Download the MySQL Installer**: Get the appropriate installer for your OS from [MySQL downloads](https://www.mysql.com/downloads/).
2. **Install MySQL**:
   - For Windows, use `mysql-installer-web-community-8.0.30.0.msi` and follow the installation steps.
3. **Start MySQL**:
   - Open a command line terminal.
   - Start MySQL with `mysql -u root -p` and enter your password.

### Setting Up the Database
1. **Create a Database**:
   ```sql
   CREATE DATABASE mydatabase;
   ```
   - Check available databases with `SHOW DATABASES;`.

### Installing MySQL DB API Driver
1. **Install `mysqlclient`**:
   ```bash
   pip3 install mysqlclient
   ```

### Enabling MySQL in Django
1. **Configure `settings.py`**:
   - Replace the default SQLite configuration with MySQL settings.
   ```python
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': 'mydatabase',
           'USER': 'root',
           'PASSWORD': '',
           'HOST': '127.0.0.1',
           'PORT': '3306',
           'OPTIONS': {
               'init_command': "SET sql_mode='STRICT_TRANS_TABLES'"
           }
       }
   }
   ```

### Optional Parameters
- **sql_mode**: Defaults to `'STRICT_TRANS_TABLES'` to prevent invalid or missing values.
- **default-character-set**: Default is `utf8`.
- **read_default_file**: Specifies a MySQL configuration file.
- **init_command**: Command to run upon connection.

### Creating Database Tables
1. **Run Migrations**:
   - Django apps like `admin`, `auth`, and `sessions` require database tables.
   - Run `python manage.py migrate` to create these tables in MySQL.
2. **Verify Tables in MySQL**:
   ```sql
   USE mydatabase;
   SHOW TABLES;
   ```

### Using VS Code MySQL Extension
1. **Install MySQL Extension**:
   - Search for MySQL in the VS Code extension marketplace and install it.
2. **Configure Connection**:
   - Click the `+` button in MySQL Explorer and enter:
     - **Domain name**: `localhost`
     - **User**: `root`
     - **Password**: (your password)
3. **Fix Authentication Issues**:
   - If you encounter authentication errors, run:
     ```sql
     ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
     FLUSH PRIVILEGES;
     ```

### Conclusion
By following these steps, you can configure and connect a MySQL database with your Django project, leveraging MySQL's advantages for scalable and robust applications.

# Setting up a MySQL connection

### Setting Up MySQL with Django

Django is a popular open-source web framework known for its rich set of tools that support easy scalability and rapid development. One of the databases Django supports is MySQL, which is especially useful when dealing with heavy data loads. Here’s a structured guide to setting up a MySQL database with a Django project.

### Benefits of MySQL
- **Open Source**: Free to use with extensive documentation.
- **Reliability**: Efficient data storage in memory.
- **Scalability**: Handles small to vast amounts of data.
- **Security**: Provides encrypted password protection and verification.

### Using Django’s ORM
Django uses an Object-Relational Mapper (ORM) to map data between the database and the application model without writing SQL queries. This makes it easier to interact with databases like MySQL.

### Installing MySQL
1. **Download and Install**:
   - For Windows: Download the installer from the [MySQL website](https://www.mysql.com/downloads/) and follow the installation steps.
   - For macOS: Use Homebrew to install MySQL by running:
     ```bash
     brew install mysql
     ```

### Setting Up MySQL
1. **Start MySQL**:
   ```bash
   mysql -u root -p
   ```
   - Enter the password set during installation.
2. **Create a Database**:
   ```sql
   CREATE DATABASE feedback;
   SHOW DATABASES;
   ```
   - This shows all databases including the new one named `feedback`.

3. **Create a User**:
   ```sql
   CREATE USER 'admindjango' IDENTIFIED BY 'password';
   GRANT ALL PRIVILEGES ON feedback.* TO 'admindjango';
   FLUSH PRIVILEGES;
   ```

### Installing MySQL Connector
1. **Install `mysqlclient`**:
   ```bash
   pip3 install mysqlclient
   ```
   - This library allows MySQL to interact with Django’s ORM.

### Configuring Django to Use MySQL
1. **Modify `settings.py`**:
   - Replace the default SQLite settings with MySQL settings.
   ```python
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': 'feedback',
           'USER': 'admindjango',
           'PASSWORD': 'password',
           'HOST': '127.0.0.1',
           'PORT': '3306',
           'OPTIONS': {
               'init_command': "SET sql_mode='STRICT_TRANS_TABLES'"
           }
       }
   }
   ```

### Running Migrations
1. **Make Migrations**:
   ```bash
   python3 manage.py makemigrations
   ```
2. **Apply Migrations**:
   ```bash
   python3 manage.py migrate
   ```
   - This will create the necessary tables in the MySQL database based on your Django models.

### Additional Tips
- **VS Code MySQL Extension**: To manage your MySQL database within Visual Studio Code, install the MySQL extension from the marketplace.
- **Authentication Issues**: If you encounter issues, run:
  ```sql
  ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
  FLUSH PRIVILEGES;
  ```

### Summary
By following these steps, you can configure and connect a MySQL database with your Django project, leveraging MySQL’s scalability and robust features for handling larger data loads. This setup allows you to utilize Django’s powerful ORM for efficient database management without the need for extensive SQL knowledge.

# Exercise : Connecting to a Database

### **Goal**

- Learner will create and connect to a MySQL database that can be used inside a Django project.

### **Objectives**

- Learner will create new MySQL database credentials.
- Learner will update the project settings in Django to enable connection with MySQL.

# Module summary: Models

### Module Recap: Models, Migrations, Forms, Admin, and MySQL Configuration in Django

#### Introduction
This module provided a comprehensive overview of essential Django functionalities including models, migrations, forms, the admin interface, and configuring a MySQL database. Here’s a structured summary of the key points and skills you have acquired.

### Models and Migrations
1. **Understanding Models**:
   - **Models**: Represent the structure of your database.
   - **Fields**: Different data types for model attributes (e.g., CharField, IntegerField).
   - **Keys**: Primary and foreign keys to establish relationships between models.

2. **Creating Models**:
   - How to define models in Django using the `models.Model` class.
   - Adding various attributes to models and understanding their purposes.

3. **Migrations**:
   - **Concept**: Migrations are a way to propagate changes made to models into the database schema.
   - **Benefits**: Simplifies database schema changes, avoids direct SQL queries, and manages changes across development teams.
   - **Best Practices**: Applying, rolling back, and managing migrations efficiently.

### Object-Relational Mapping (ORM) and QuerySet API
1. **ORM**:
   - Django’s ORM translates Python code into SQL queries, allowing developers to interact with the database without writing SQL directly.
   - Facilitates database operations such as CRUD (Create, Read, Update, Delete).

2. **QuerySet API**:
   - **Saving Data**: Using `save()` method on model instances.
   - **Retrieving Data**: Using methods like `filter()`, `all()`, `get()`, and `exclude()` to query the database.

### Forms in Django
1. **Forms Framework**:
   - Handling form data with Django’s form classes.
   - Binding data from requests to form objects and validating the data.

2. **Creating Forms**:
   - Using Django’s form API to create and manage forms.
   - Defining different form fields and their validations.

### Django Admin Interface
1. **Admin Panel**:
   - Accessing and customizing the Django admin interface to manage the application.

2. **User and Group Management**:
   - Creating and managing users and groups.
   - Setting up and controlling permissions at the view, model, and controller levels.

3. **Permissions**:
   - Adding, modifying, and revoking permissions using the admin interface.

### Database Configuration
1. **Database Options**:
   - Overview of databases supported by Django: SQLite, MySQL, PostgreSQL, etc.
   - SQLite for local development and rapid prototyping.

2. **Setting Up MySQL**:
   - **Installation**: Installing MySQL server and client.
   - **Configuration**: Updating `settings.py` with MySQL database settings.
   - **Database Connection**: Establishing and testing the MySQL database connection.

### Key Skills Acquired
- **Model Creation**: Defining models with various fields and relationships.
- **Applying Migrations**: Managing database schema changes efficiently.
- **Using QuerySet API**: Interacting with the database using Django’s ORM.
- **Form Handling**: Creating and managing forms using Django’s forms framework.
- **Admin Interface**: Leveraging Django’s admin panel for user and permission management.
- **MySQL Configuration**: Setting up and configuring a MySQL database for a Django project.

### Conclusion
You’ve successfully navigated through creating models, managing migrations, handling forms, utilizing the admin panel, and configuring a MySQL database in Django. These foundational skills are crucial for building robust and scalable web applications using Django. Keep up the excellent progress in your Django development journey!

# Additional resources
The following resources will be helpful as additional references in dealing with different concepts related to the topics you have covered in Database Configuration.

Databases – Django official
- https://docs.djangoproject.com/en/4.1/ref/databases/

MySQL notes – Django official
- https://docs.djangoproject.com/en/4.1/ref/databases/#mysql-notes

Installing MySQL Client
- https://docs.djangoproject.com/en/4.1/ref/databases/#mysql-db-api-drivers

Installing MySQL on MacOS
- https://dev.mysql.com/doc/refman/5.7/en/macos-installation.html

Installing MySQL on Windows
- https://dev.mysql.com/doc/refman/5.7/en/windows-installation.html

Installing and configuring MySQL with Django
- https://docs.djangoproject.com/en/4.1/ref/databases/#mysql-notes


