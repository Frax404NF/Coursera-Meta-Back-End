# Django Admin

### Summary: Django Admin Interface

In this tutorial, you will learn how to activate and use Django's admin interface, a powerful feature that allows you to manage your application's data through a user-friendly interface. This admin site is designed for site managers and administrators, not for regular site visitors.

#### Activating the Admin Interface

1. **Setup and Configuration**:
   - Django’s admin interface is enabled by default when you start a new Django project.
   - The admin site is linked to the models in your project once registered.
   - Ensure `django.contrib.admin` is listed in the `INSTALLED_APPS` section of your `settings.py` file.

2. **Creating a Superuser**:
   - To access the admin interface, you need to create an admin user with the necessary credentials.
   - Run the following command in the terminal to create a superuser:
   
     ```bash
     python manage.py createsuperuser
     ```
   - Follow the prompts to enter a username, email address, and password.

3. **Running the Server**:
   - Start the development server by running:
   
     ```bash
     python manage.py runserver
     ```
   - Navigate to `http://127.0.0.1:8000/admin` to access the admin login page.
   - Log in using the credentials of the superuser you created.

#### Using the Admin Interface

1. **Admin Dashboard**:
   - After logging in, you will see the admin dashboard, which provides an overview of the groups and users, along with any registered models.
   - The dashboard also shows recent actions performed in the admin interface.

2. **Managing Models**:
   - Click on any registered model (e.g., `Reservations`) to view, add, or modify entries.
   - You can update existing entries, add new entries, or delete selected entries using the provided forms.

3. **User and Group Management**:
   - The admin interface allows you to manage users and groups.
   - For a specific user, you can change the username, password, personal info, and assign them to groups.
   - You can also designate specific permissions for users and view their activity history.

4. **Permissions and Groups**:
   - You can create groups and assign permissions to them, making it easier to manage access control for multiple users.
   - Permissions can be customized for different actions (e.g., add, change, delete) on various models.

#### Benefits of Using Django Admin

- **Automated Interface**: The admin site automatically generates a management interface based on your models, saving you from building an admin panel from scratch.
- **Time-Saving**: Provides a ready-to-use interface for managing users, groups, and data, reducing development time.
- **Security**: Ensures that administrative tasks are performed securely with proper authentication and permission controls.
- **Flexibility**: Easily customizable to fit the needs of your application, including adding custom views and modifying existing ones.

#### Conclusion

By following this tutorial, you have learned how to:

- Activate and configure Django's admin interface.
- Create a superuser to access the admin site.
- Use the admin dashboard to manage users, groups, and model data.
- Assign permissions and manage user groups effectively.

Django’s admin interface is a powerful tool that simplifies data management tasks and provides a robust, secure, and customizable interface for administrators. As you continue developing your application, leveraging the Django admin panel will significantly enhance your productivity and ease of management.

# Managing users in Django Admin

### Managing Users in Django Admin

Django’s admin interface, part of the `django.contrib.admin` app, is a powerful tool for managing users and groups within a web application. This tutorial will guide you through customizing the admin interface to better control user permissions and how models are displayed.

#### Setting Up the Admin Interface

1. **Ensure `INSTALLED_APPS` Configuration**:
   - Make sure `django.contrib.admin` is listed in the `INSTALLED_APPS` section of your `settings.py` file.

2. **Creating a Superuser**:
   - To access the admin interface, you need a superuser. Create one with the following command:
   
     ```bash
     python manage.py createsuperuser
     ```
   - Follow the prompts to enter a username, email address, and password.

3. **Accessing the Admin Interface**:
   - Start the server:
   
     ```bash
     python manage.py runserver
     ```
   - Navigate to `http://127.0.0.1:8000/admin` and log in with your superuser credentials.

#### Customizing User Permissions

1. **Unregistering and Registering User Admin**:
   - Unregister the default user admin and register a custom one to better control permissions.
   
     ```python
     from django.contrib import admin
     from django.contrib.auth.models import User
     from django.contrib.auth.admin import UserAdmin

     # Unregister the default User admin
     admin.site.unregister(User)

     # Register a custom User admin
     @admin.register(User)
     class CustomUserAdmin(UserAdmin):
         pass
     ```

2. **Making Fields Read-Only**:
   - To make certain fields read-only, modify the `CustomUserAdmin` class.
   
     ```python
     @admin.register(User)
     class CustomUserAdmin(UserAdmin):
         readonly_fields = ['date_joined']
     ```

3. **Controlling Field Access Based on User Type**:
   - Override the `get_form` method to restrict staff users from editing certain fields.
   
     ```python
     @admin.register(User)
     class CustomUserAdmin(UserAdmin):
         def get_form(self, request, obj=None, **kwargs):
             form = super().get_form(request, obj, **kwargs)
             if not request.user.is_superuser:
                 form.base_fields['username'].disabled = True
             return form
     ```

#### Managing Models in the Admin Interface

1. **Adding and Registering Models**:
   - Add a model in your `models.py` file.
   
     ```python
     from django.db import models

     class Person(models.Model):
         last_name = models.TextField()
         first_name = models.TextField()

         def __str__(self):
             return f"{self.last_name}, {self.first_name}"
     ```
   - Register the model in `admin.py`.
   
     ```python
     from django.contrib import admin
     from .models import Person

     @admin.register(Person)
     class PersonAdmin(admin.ModelAdmin):
         list_display = ("last_name", "first_name")
         search_fields = ("first_name__startswith",)
     ```

2. **Customizing Model Display**:
   - Customize how the model is displayed using `list_display` and `search_fields`.
   
     ```python
     @admin.register(Person)
     class PersonAdmin(admin.ModelAdmin):
         list_display = ("last_name", "first_name")
         search_fields = ("first_name__startswith",)
     ```

3. **Testing Changes**:
   - Log in to the admin interface with the superuser credentials.
   - Check the `Person` model to ensure it displays correctly with the desired customizations.

### Conclusion

By following these steps, you have learned how to:

- Customize Django’s admin interface to manage user permissions more effectively.
- Make specific fields read-only.
- Control access to fields based on user type.
- Register and display models with custom settings in the admin interface.

Django’s admin panel is a powerful tool that, when customized correctly, can greatly enhance the security and usability of your web application.

# Adding groups and users

In this tutorial, we will create a reservation system for the front desk of Little Lemon using Django. We'll cover how to register a model in `admin.py` and manage users and groups using the Django administration interface. Employees have been manually entering reservation details, but we aim to make the process more efficient.

#### Step-by-Step Guide

1. **Setting Up the Models**:
   - Open `models.py` and define the `Reservation` model with five attributes: `name`, `contact`, `time`, `guests`, and `notes`.

     ```python
     from django.db import models

     class Reservation(models.Model):
         name = models.CharField(max_length=100)
         contact = models.CharField(max_length=100)
         time = models.DateTimeField()
         guests = models.IntegerField()
         notes = models.TextField()

         def __str__(self):
             return self.name
     ```

2. **Adding the App to Installed Apps**:
   - Open `settings.py` and ensure your app is added to the `INSTALLED_APPS` list.

     ```python
     INSTALLED_APPS = [
         ...
         'myapp',
     ]
     ```

3. **Making Migrations**:
   - Create the necessary migrations for the `Reservation` model.

     ```bash
     python manage.py makemigrations
     python manage.py migrate
     ```

4. **Creating a Superuser**:
   - Create a superuser to access the admin interface.

     ```bash
     python manage.py createsuperuser
     ```

     - Enter a username, email, and password.

5. **Registering the Model in Admin**:
   - Open `admin.py` and register the `Reservation` model.

     ```python
     from django.contrib import admin
     from .models import Reservation

     @admin.register(Reservation)
     class ReservationAdmin(admin.ModelAdmin):
         list_display = ("name", "contact", "time", "guests", "notes")
         search_fields = ("name__startswith", "contact")
     ```

6. **Running the Server**:
   - Start the server and access the admin interface.

     ```bash
     python manage.py runserver
     ```

   - Navigate to `http://127.0.0.1:8000/admin` and log in with the superuser credentials.

7. **Adding Reservations**:
   - In the admin interface, navigate to the `Reservations` section and add new reservations.
   - Fill in the form fields: name, contact, time, number of guests, and any additional notes.

8. **Customizing User Management**:
   - **Add Users**: Navigate to the Users section, add a new user, and provide a username and password.
   - **Set Permissions**: Assign permissions such as active, staff, or superuser.
   - **Create Groups**: Create user groups and assign permissions to manage which users can access the admin panel and make reservations.

9. **Testing and Verifying**:
   - Log in as different users and test the access and permissions.
   - Ensure that only authorized users can make changes to reservations.

10. **Previewing the Database**:
    - Use tools like SQLite Explorer in VS Code to preview the database and verify that the reservations are being saved correctly.

    ```python
    # Example view in SQLite Explorer:
    # myapp > Reservations > Show table
    ```

By following these steps, you have created a basic reservation system in Django for Little Lemon, allowing employees to efficiently manage reservations through the Django administration interface. This system improves upon the manual entry method and provides a more robust solution for handling reservations.

# Permissions

### Understanding User and Model Permissions in Django

Django has a robust system for handling authentication and authorization, making it easy to control which users are allowed to perform certain actions. Here's a detailed guide on user and model permissions in Django.

#### User Classifications in Django

1. **Superuser**:
   - Top-level user with all permissions.
   - Can add, change, and delete other users and perform any operation on the data.

2. **Staff User**:
   - Can access the Django admin interface.
   - Does not have automatic permission to create, read, update, or delete data; these must be explicitly granted.
   - A superuser is a staff user by default.

3. **Regular User**:
   - Cannot access the admin site.
   - Marked as active by default, but can be inactive if authentication fails or the user is banned.

#### Creating Users

1. **Creating a Superuser**:
   - Use the `createsuperuser` command.

     ```bash
     python manage.py createsuperuser
     ```

2. **Creating Users through the Admin Interface**:
   - Navigate to the admin interface, go to the "Users" section, and click "Add user."
   - Fill in the username and password.
   - Set additional details and permissions (e.g., active, staff status).

3. **Creating Users through the Django Shell**:

   ```python
   from django.contrib.auth.models import User

   # Create a regular user
   user = User.objects.create_user(username='john', password='password')

   # Grant staff status
   user.is_staff = True
   user.save()
   ```

#### Setting Permissions in Models

Django automatically creates basic permissions (`add`, `change`, `delete`, `view`) for each model. Permissions follow the pattern: `app_label.action_modelname`.

**Example**: For a model named `MyModel` in the app `myapp`:

- `myapp.add_mymodel`
- `myapp.change_mymodel`
- `myapp.delete_mymodel`
- `myapp.view_mymodel`

You can define additional permissions in the model's `Meta` class:

```python
class MyModel(models.Model):
    ...

    class Meta:
        permissions = [
            ("can_publish", "Can publish articles"),
            ("can_approve", "Can approve comments"),
        ]
```

#### Checking Permissions

Use the `has_perm` method to check if a user has a specific permission:

```python
if request.user.has_perm('myapp.change_mymodel'):
    # The user has permission
else:
    raise PermissionDenied
```

#### Using Groups for Permissions

Groups allow you to manage permissions for sets of users efficiently.

1. **Creating Groups**:
   - Navigate to the admin interface, go to the "Groups" section, and click "Add group."
   - Name the group and assign the necessary permissions.

2. **Assigning Users to Groups**:
   - Edit the user in the admin interface.
   - Select the desired groups for the user.

**Example**: Assign permissions to kitchen staff and waiters.

- Create a group `Kitchen Staff` with permissions to manage kitchen-related models.
- Create a group `Waiters` with permissions to manage reservation-related models.
- Assign users to these groups as appropriate.

#### Enforcing Permissions in Views

You can enforce permissions in views using decorators like `permission_required`.

```python
from django.contrib.auth.decorators import permission_required

@permission_required('myapp.change_mymodel', raise_exception=True)
def my_view(request):
    # The user has the 'change_mymodel' permission
    ...
```

#### Conclusion

Django’s permissions system is comprehensive and flexible, allowing fine-grained control over what actions users can perform. By understanding and utilizing user roles, model permissions, and groups, you can create a secure and well-organized application. Whether through the admin interface or the shell, Django makes it straightforward to manage user permissions and ensure that your application operates smoothly and securely.

# Enforcing Permissions

### Enforcing Permissions in Django

Permissions are a crucial part of any web application to control which users can perform certain actions. Django provides various ways to enforce permissions at different levels, such as the admin interface, views, templates, and URL patterns. Let's explore how to implement and enforce these permissions.

#### Model Permissions in the Admin Interface

Permissions can be granted through the Django Admin interface. By default, Django creates `add`, `change`, `view`, and `delete` permissions for each model. Custom permissions can also be defined in the model’s `Meta` class.

Example:
```python
class Product(models.Model):
    product_id = models.IntegerField()
    name = models.TextField()
    category = models.TextField()
    
    class Meta:
        permissions = [
            ('can_change_category', 'Can change category'),
        ]
```

#### Enforcing Permissions at the View Level

1. **Checking Permissions Manually**:
   ```python
   from django.core.exceptions import PermissionDenied

   def myview(request):
       if request.user.is_anonymous:
           raise PermissionDenied()
   ```

2. **Using `login_required` Decorator**:
   Ensures that only authenticated users can access the view.
   ```python
   from django.http import HttpResponse
   from django.contrib.auth.decorators import login_required

   @login_required
   def myview(request):
       return HttpResponse("Hello World")
   ```

3. **Using `user_passes_test` Decorator**:
   Takes a function that returns `True` or `False` based on custom logic.
   ```python
   from django.contrib.auth.decorators import user_passes_test

   def testpermission(user):
       return user.is_authenticated and user.has_perm("myapp.change_category")

   @user_passes_test(testpermission)
   def change_ctg(request):
       # Logic for changing category of product model instance
       pass
   ```

4. **Using `permission_required` Decorator**:
   Restricts access based on specific permissions.
   ```python
   from django.contrib.auth.decorators import permission_required

   @permission_required("myapp.change_category")
   def store_creator(request):
       # Logic for changing category of product model instance
       pass
   ```

#### Enforcing Permissions in Class-Based Views

Use `PermissionRequiredMixin` to enforce permissions in class-based views.

Example:
```python
from django.contrib.auth.mixins import PermissionRequiredMixin
from django.views.generic import ListView
from .models import Product

class ProductListView(PermissionRequiredMixin, ListView):
    permission_required = "myapp.view_product"
    template_name = "product.html"
    model = Product
```

#### Enforcing Permissions in Templates

Django templates provide `user` and `perms` variables to check user attributes and permissions.

Example:
```html
<html>
<body>
{% if user.is_authenticated %}
    <!-- Content for authenticated users -->
{% endif %}

{% if perms.myapp.change_category %}
    <!-- Content for users with change_category permission -->
{% endif %}
</body>
</html>
```

#### Enforcing Permissions in URL Patterns

Permissions can be enforced at the URL level using decorators.

Example:
```python
from django.conf.urls import url
from django.contrib.auth.decorators import login_required, permission_required

urlpatterns = [
    url(r'^users_only/', login_required(myview)),
    url(r'^category/', permission_required('myapp.change_category', login_url='login')(myview)),
]
```

### Summary

Django offers multiple ways to enforce permissions:
- **Admin Interface**: Grant permissions directly through the admin panel.
- **View Level**: Use decorators like `login_required`, `user_passes_test`, and `permission_required`.
- **Class-Based Views**: Utilize `PermissionRequiredMixin`.
- **Templates**: Check user attributes and permissions using template tags.
- **URL Patterns**: Apply permission checks directly in URL configurations.

These methods ensure that your application remains secure by controlling access to various parts based on user roles and permissions.

# Users and Permissions

### Managing User Permissions in Django

Django provides a built-in mechanism for handling permissions, which can be managed through both the Django admin interface and the Django shell. Here’s a comprehensive guide on how to create users, assign permissions, and manage groups.

#### Using the Django Shell to Manage Permissions

1. **Creating a User**:
   - Open the Django shell by running:
     ```shell
     python manage.py shell
     ```
   - Import the `User` model:
     ```python
     from django.contrib.auth.models import User
     ```
   - Create a new user:
     ```python
     user = User.objects.create_user(username='mario', email='mario@example.com', password='password123')
     ```
   - Retrieve an existing user by username:
     ```python
     user = User.objects.get(username='mario')
     ```

2. **Assigning Permissions to a User**:
   - Import the `Permission` model:
     ```python
     from django.contrib.auth.models import Permission
     ```
   - Retrieve a specific permission (e.g., `change_category` for a `Product` model in `myapp`):
     ```python
     permission = Permission.objects.get(codename='can_change_category')
     ```
   - Add the permission to the user:
     ```python
     user.user_permissions.add(permission)
     ```

3. **Verifying Permissions**:
   - Check if the user has a specific permission:
     ```python
     user.has_perm('myapp.can_change_category')
     ```

#### Managing Permissions Using the Django Admin Interface

1. **Creating Groups**:
   - Log into the Django admin interface with a superuser account.
   - Navigate to the `Groups` section under `Authentication and Authorization`.
   - Click `Add Group` and enter the group name (e.g., `Reservation Desk`).
   - In the `Available permissions` table, select the relevant permissions and add them to the group.

2. **Creating Users**:
   - Navigate to the `Users` section under `Authentication and Authorization`.
   - Click `Add User` and fill out the required fields (username, password).
   - After creating the user, fill out additional information (first name, last name, email).
   - Assign the user to a group by selecting from the `Groups` field.
   - Set user permissions and status (e.g., active, staff status, superuser status).

3. **Assigning Permissions to Groups**:
   - Groups allow batch management of permissions.
   - Assign a user to a group to automatically inherit all permissions assigned to that group.

#### Example Scenario

**Creating Users and Groups:**

1. **Creating Groups:**
   - Create `Reservation Desk` group:
     - Go to `Groups`, click `Add Group`.
     - Name: `Reservation Desk`.
     - Assign permissions for the `Reservation` model (add, change, delete, view).
   - Create `Admin Users` group:
     - Go to `Groups`, click `Add Group`.
     - Name: `Admin Users`.
     - Assign all permissions by clicking `Choose all`.

2. **Creating Users:**
   - Create user `Mario`:
     - Go to `Users`, click `Add User`.
     - Username: `Mario`, Password: `password123`.
     - Assign `Admin Users` group, set as superuser.
   - Create user `Maher`:
     - Go to `Users`, click `Add User`.
     - Username: `Maher`, Password: `password123`.
     - Assign `Reservation Desk` group, set as staff.

**Filtering and Managing Users:**

- Filter users by status or group in the Django admin.
- Use the `Recent Actions` section to track changes.

### Summary

Django’s permission system is robust and flexible, allowing you to manage permissions through both the admin interface and the Django shell. By creating users, assigning them to groups, and managing permissions effectively, you can control access to different parts of your application efficiently. Whether you’re handling permissions for individual users or groups, Django makes it straightforward to ensure that only authorized users can perform specific actions.

# Exercise: Using Django Admin

Goal
- Learner will learn to use the Django admin panel.

Objectives
- Learner will create a super user that will have administrative rights.

- Learner will create an entry for a specific user and create another new user and assign specific permissions to it using the admin panel of Django. 

# Additional resources
The following resources will be helpful as additional references in dealing with different concepts related to Django admin, Django-admin and manage.py commands and permissions.

django-admin and manage.py – official documentation
- https://docs.djangoproject.com/en/4.1/ref/django-admin/

Using the Django authentication system
- https://docs.djangoproject.com/en/4.1/topics/auth/default/

Django Admin site – MDN web docs
- https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Admin_site

Django Admin-site – Comprehensive
- https://docs.djangoproject.com/en/4.1/ref/contrib/admin/

Django permissions – TestDriven
- https://docs.djangoproject.com/en/4.1/topics/auth/customizing/