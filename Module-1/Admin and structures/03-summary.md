# Django-admin & manage.py commands

### Django Admin vs. Manage.py: Understanding Their Roles and Usage

When developing with Django, you have two command-line utilities at your disposal: `django-admin` and `manage.py`. Both serve to streamline the development process by allowing you to perform various administrative tasks, but they have some differences and specific use cases. This guide will help you understand these tools and how to use them effectively.

#### Overview of Django Admin and Manage.py

**Django Admin:**
- A command-line utility for performing administrative tasks.
- Installed globally with Django and available in the scripts folder of your Django environment.
- Useful for tasks that span across multiple projects or when switching between different project settings.

**Manage.py:**
- A project-specific command-line utility that is automatically generated within each Django project.
- Essentially a wrapper around `django-admin` that sets the `DJANGO_SETTINGS_MODULE` environment variable to point to your project's `settings.py` file.
- Preferred for most tasks when working within a single project.

#### Creating a Django Project

To create a Django project, you can use either `django-admin` or `manage.py`. Here’s how you can do it:

1. **Using `django-admin`**:
   ```shell
   django-admin startproject myproject
   ```

2. **Using `manage.py`**:
   - First, create the project using `django-admin` (since `manage.py` is generated as part of the project creation):
     ```shell
     django-admin startproject myproject
     ```
   - Navigate into the project directory and then use `manage.py` for subsequent tasks.

#### Running the Development Server

Once you have created your project, you will want to run the development server to start building your application.

1. **Using `manage.py`**:
   ```shell
   python manage.py runserver
   ```

2. **Using `django-admin`**:
   - Set the `DJANGO_SETTINGS_MODULE` environment variable to point to your project's settings:
     ```shell
     DJANGO_SETTINGS_MODULE=myproject.settings django-admin runserver
     ```

#### Common Commands

Here are some common commands you will use frequently with `manage.py`:

- **Creating an App**:
  ```shell
  python manage.py startapp myapp
  ```

- **Making Migrations**:
  ```shell
  python manage.py makemigrations
  ```

- **Applying Migrations**:
  ```shell
  python manage.py migrate
  ```

- **Opening the Django Shell**:
  ```shell
  python manage.py shell
  ```

#### Key Differences and Use Cases

**Using `manage.py`**:
- Preferred when working within a single project.
- Automatically uses the correct settings file from your project.
- Simplifies command usage since you don't need to set the `DJANGO_SETTINGS_MODULE` manually.

**Using `django-admin`**:
- Useful when you need to perform tasks that are not project-specific.
- Can be used to switch between multiple projects by setting the `DJANGO_SETTINGS_MODULE`.
- Handy for global administrative tasks.

### Example Workflow

Let's walk through an example workflow of setting up a Django project, creating an app, making migrations, and running the development server.

1. **Create a Project Directory**:
   ```shell
   mkdir myproject
   cd myproject
   ```

2. **Set Up a Virtual Environment**:
   ```shell
   python3 -m venv env
   source env/bin/activate   # On Windows: env\Scripts\activate
   ```

3. **Install Django**:
   ```shell
   pip install django
   ```

4. **Create a Django Project**:
   ```shell
   django-admin startproject myproject .
   ```

5. **Create an App**:
   ```shell
   python manage.py startapp myapp
   ```

6. **Make Migrations**:
   ```shell
   python manage.py makemigrations
   ```

7. **Apply Migrations**:
   ```shell
   python manage.py migrate
   ```

8. **Run the Development Server**:
   ```shell
   python manage.py runserver
   ```

#### Conclusion

Understanding the roles and usage of `django-admin` and `manage.py` helps streamline your Django development process. While both tools offer similar functionalities, `manage.py` is more convenient for project-specific tasks due to its automatic configuration of project settings. By following best practices and using these tools effectively, you can efficiently manage your Django projects and focus on building robust web applications.

# App structures

### Understanding the Django App Structure

Django projects consist of multiple sub-applications, known as apps, each responsible for a distinct aspect of the web application. Here’s a detailed guide on the structure of a Django app, how to create one, and how to configure project settings to include the app.

#### What is a Django App?

- **Modular Approach**: An app in Django is a self-contained module that performs a single task. For example, in a trading organization website, you might have separate apps for customer data management, supplier management, and stock management.
- **Reusability**: Apps are reusable. You can use an app from one Django project in another without modification.

#### Creating a Django App

When you create a Django project using the `startproject` command, it sets up a basic project structure. You then use the `startapp` command to create individual apps within this project.

1. **Creating the App**:
   ```shell
   python manage.py startapp demoapp
   ```

   This command creates a new folder named `demoapp` with the following default structure:
   ```plaintext
   demoproject/
   ├── db.sqlite3
   ├── manage.py
   ├── demoapp/
   │   ├── admin.py
   │   ├── apps.py
   │   ├── models.py
   │   ├── tests.py
   │   └── views.py
   └── demoproject/
       ├── __init__.py
       ├── settings.py
       ├── urls.py
       └── wsgi.py
   ```

#### Understanding the App Structure

- **views.py**: Contains view functions that handle requests and return responses. Initially empty, you can define functions like this:
  ```python
  from django.http import HttpResponse

  def index(request):
      return HttpResponse("Hello, world. This is the index view of Demoapp.")
  ```

- **urls.py**: Defines URL patterns for the app. This file is not created by default, so you need to create it manually:
  ```python
  from django.urls import path
  from . import views

  urlpatterns = [
      path('', views.index, name='index'),
  ]
  ```

- **models.py**: Where you define data models (database schemas). Initially empty.

- **admin.py**: Register your models here to make them visible in the Django admin interface.

- **tests.py**: For writing tests for your app.

- **apps.py**: Contains configuration for the app.

#### Configuring the Project to Include the App

1. **Update Project’s urls.py**: Include the app's URL configurations.
   ```python
   from django.contrib import admin
   from django.urls import include, path

   urlpatterns = [
       path('demo/', include('demoapp.urls')),
       path('admin/', admin.site.urls),
   ]
   ```

2. **Update settings.py**: Add the app to the `INSTALLED_APPS` list.
   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'demoapp',
   ]
   ```

#### Running the Development Server

After configuring the app, you can run the development server:
```shell
python manage.py runserver
```

Visit `http://localhost:8000/demo/` in your browser. You should see the message "Hello, world. This is the index view of Demoapp."

#### Additional Files in the App Structure

Apart from the default files, you might also encounter or create additional files in your app for specific functionalities:
- **forms.py**: For defining forms.
- **serializers.py**: For defining serializers in case you're using Django REST framework.

### Summary

In this guide, you learned how to:
1. Create a Django app using `startapp`.
2. Understand the structure and purpose of files in the app directory.
3. Configure project settings to include the new app.
4. Add a simple view and URL routing for the app.
5. Run the development server and test the app.

By following these steps, you can modularize your Django project effectively, making your codebase cleaner and more maintainable.

# Creating an App

### Creating an App in an Existing Django Project

In this guide, you'll learn how to create an app within an existing Django project, set up a view to output text to the homepage, and configure the necessary URL mappings.

#### Step-by-Step Instructions

1. **Create a Django App**
   - Open your terminal in the project directory.
   - Run the `startapp` command to create a new app named `myapp`:
     ```bash
     python -m django startapp myapp
     ```
   - This command generates a new folder called `myapp` inside your project directory, containing several default files (e.g., `admin.py`, `apps.py`, `models.py`, `tests.py`, `views.py`).

2. **Create a View Function**
   - Open the `views.py` file located in the `myapp` folder.
   - Add a function named `home` that returns an HTTP response:
     ```python
     from django.http import HttpResponse

     def home(request):
         return HttpResponse("Hello, World!")
     ```

3. **Create URL Configuration for the App**
   - Create a new file named `urls.py` inside the `myapp` folder.
   - Define the URL patterns to map URLs to the view functions:
     ```python
     from django.urls import path
     from . import views

     urlpatterns = [
         path('', views.home, name='home'),
     ]
     ```

4. **Update Project’s URL Configuration**
   - Open the `urls.py` file in the project’s main directory.
   - Include the app’s URL configurations:
     ```python
     from django.contrib import admin
     from django.urls import include, path

     urlpatterns = [
         path('myapp/', include('myapp.urls')),
         path('admin/', admin.site.urls),
     ]
     ```

5. **Update Project Settings**
   - Open the `settings.py` file in the project directory.
   - Add `myapp` to the `INSTALLED_APPS` list:
     ```python
     INSTALLED_APPS = [
         'django.contrib.admin',
         'django.contrib.auth',
         'django.contrib.contenttypes',
         'django.contrib.sessions',
         'django.contrib.messages',
         'django.contrib.staticfiles',
         'myapp',
     ]
     ```

6. **Run the Development Server**
   - Start the development server to test your changes:
     ```bash
     python manage.py runserver
     ```
   - Visit `http://localhost:8000/myapp/` in your browser. You should see the text "Hello, World!" displayed on the webpage.

#### Summary of Key Concepts

- **App Creation**: Using `startapp` to create a new app within the project.
- **Views**: Functions in `views.py` that handle HTTP requests and return responses.
- **URL Routing**: Mapping URLs to view functions using `urls.py`.
- **Project Configuration**: Updating `settings.py` and the main `urls.py` to include the new app.

### Example Code

**views.py**
```python
from django.http import HttpResponse

def home(request):
    return HttpResponse("Hello, World!")
```

**urls.py** (inside `myapp` folder)
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
]
```

**urls.py** (inside project folder)
```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('myapp/', include('myapp.urls')),
    path('admin/', admin.site.urls),
]
```

**settings.py** (inside project folder)
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'myapp',
]
```

By following these steps, you have successfully created an app within an existing Django project, set up a view to output text, and configured the URL routing to display the text on the homepage.

# 