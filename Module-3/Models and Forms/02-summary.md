# Forms

### Understanding Forms in Django

#### Forms and Data Collection in Web Applications

Web applications often need to collect data from users, such as login details, registration information, or order details. Traditionally, HTML forms with various input elements are used to gather this data, which is then submitted to the server for processing.

#### Challenges with HTML Forms

While HTML forms are effective, they can become cumbersome and error-prone, especially when dealing with large or complex forms. Each form element's name or ID must match the expected names in the server-side code, increasing the potential for errors.

#### Django's Form Class

To simplify form creation and processing, Django provides a `Form` class. This class allows developers to define form fields as Python class attributes, making form management easier and more reliable. Using Django's `Form` class, developers can:

- Automatically generate HTML for form elements.
- Validate form data.
- Handle form submissions with less risk of mismatched field names.

##### Creating a Simple Form

Let's create a basic form in Django. Suppose we need a form to collect a user's name:

1. **Define the Form Class**

   Create a file named `forms.py` in your Django app directory and define the form class:

   ```python
   from django import forms

   class NameForm(forms.Form):
       your_name = forms.CharField(max_length=100)
   ```

   Here, `NameForm` inherits from `forms.Form`, and `your_name` is a `CharField` with a maximum length of 100 characters.

2. **Render the Form in a View**

   In your `views.py`, import the form and create a view to render the form:

   ```python
   from django.shortcuts import render
   from .forms import NameForm

   def get_name(request):
       if request.method == "POST":
           form = NameForm(request.POST)
           if form.is_valid():
               # Process the data in form.cleaned_data
               return HttpResponseRedirect('/thanks/')
       else:
           form = NameForm()

       return render(request, 'name.html', {'form': form})
   ```

3. **Create a Template for the Form**

   Create a template named `name.html` in your templates directory:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Name Form</title>
   </head>
   <body>
       <h1>Enter your name</h1>
       <form method="post">
           {% csrf_token %}
           {{ form.as_p }}
           <button type="submit">Submit</button>
       </form>
   </body>
   </html>
   ```

   The `{{ form.as_p }}` renders the form fields as paragraph tags.

#### Model Forms

Django also provides `ModelForm`, which allows you to create forms directly from your models. This ensures that the form fields correspond to the model fields, reducing potential errors and making it easier to handle form data.

##### Creating a Model Form

1. **Define a Model**

   In your `models.py`, define a model:

   ```python
   from django.db import models

   class User(models.Model):
       name = models.CharField(max_length=100)
       email = models.EmailField()
   ```

2. **Create a ModelForm**

   In your `forms.py`, create a form for the model:

   ```python
   from django import forms
   from .models import User

   class UserForm(forms.ModelForm):
       class Meta:
           model = User
           fields = ['name', 'email']
   ```

3. **Use the ModelForm in a View**

   In your `views.py`, create a view to handle the form:

   ```python
   from django.shortcuts import render, redirect
   from .forms import UserForm

   def register_user(request):
       if request.method == "POST":
           form = UserForm(request.POST)
           if form.is_valid():
               form.save()
               return redirect('/thanks/')
       else:
           form = UserForm()

       return render(request, 'register.html', {'form': form})
   ```

4. **Create a Template for the ModelForm**

   Create a template named `register.html`:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Register User</title>
   </head>
   <body>
       <h1>Register</h1>
       <form method="post">
           {% csrf_token %}
           {{ form.as_p }}
           <button type="submit">Register</button>
       </form>
   </body>
   </html>
   ```

#### Advantages of Using Django Forms

- **Reduces Boilerplate Code**: Automatically generates form fields and handles validation.
- **Ensures Consistency**: Field names in the form match those expected in the server-side code.
- **Error Handling**: Provides built-in mechanisms to handle validation errors.
- **Reusability**: Forms can be reused across different views and templates.

By leveraging Django's `Form` and `ModelForm` classes, developers can create, validate, and process forms more efficiently, reducing the risk of errors and improving the maintainability of their code.

# Working with Django form fields and data types

### Using Form Fields in Django to Store Correct Data Types

#### Introduction to Form Fields

In web applications, forms are essential for collecting data from users. A typical HTML form contains elements like text inputs, radio buttons, drop-down lists, and checkboxes, which are used to gather user input and send it to the server for processing.

#### Defining Forms in Django

Django simplifies form creation and processing through its `Form` class. This class defines the expected attributes and methods to construct and handle the form. Form fields in Django map to various HTML input types and ensure that the correct data types are collected.

#### Common Form Fields in Django

1. **CharField**: Accepts string input, equivalent to the HTML text input element.
2. **EmailField**: Ensures the input follows the email format, similar to the HTML email input element.
3. **IntegerField**: Accepts integer values, akin to the HTML number input element.
4. **MultipleChoiceField**: Allows selection of multiple options, comparable to the HTML select and option elements.
5. **FileField**: Supports file uploads, corresponding to the HTML file input element.

#### Core Arguments for Form Fields

- **required**: Indicates whether the field is mandatory. Default is `True`.
- **label**: Sets a custom label for the field.
- **initial**: Specifies an initial value for the field.
- **help_text**: Provides descriptive text for the field.

### Creating a Customer Form

Let's create a customer form for the manager of Little Lemon restaurant. The form will collect the customer's name and age.

1. **Setting Up the Form**

   Create a `forms.py` file in your Django app directory and define the form:

   ```python
   from django import forms

   class CustomerForm(forms.Form):
       name = forms.CharField(max_length=100, label='Customer Name')
       age = forms.IntegerField(label='Customer Age')
   ```

2. **Rendering the Form in a View**

   In your `views.py`, import the form and create a view to handle the form:

   ```python
   from django.shortcuts import render
   from .forms import CustomerForm

   def customer_view(request):
       if request.method == "POST":
           form = CustomerForm(request.POST)
           if form.is_valid():
               # Process the data in form.cleaned_data
               return HttpResponseRedirect('/thanks/')
       else:
           form = CustomerForm()

       return render(request, 'customer_form.html', {'form': form})
   ```

3. **Creating a Template for the Form**

   Create a template named `customer_form.html`:

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Customer Form</title>
   </head>
   <body>
       <h1>Enter Customer Details</h1>
       <form method="post">
           {% csrf_token %}
           {{ form.as_p }}
           <button type="submit">Submit</button>
       </form>
   </body>
   </html>
   ```

### Exploring Various Form Fields

1. **CharField with TextArea Widget**

   ```python
   from django import forms

   class DemoForm(forms.Form):
       description = forms.CharField(widget=forms.Textarea, initial='Enter description here', help_text='Describe yourself in 100 words.')
   ```

   - `widget=forms.Textarea`: Renders a text area instead of a text input.
   - `initial`: Sets an initial value for the field.
   - `help_text`: Provides additional information for the user.

2. **EmailField with Validation**

   ```python
   class DemoForm(forms.Form):
       email = forms.EmailField(label='Enter your email address', help_text='We will not share your email.')
   ```

   - Validates the input to ensure it is a valid email address.

3. **DateField with Calendar Widget**

   ```python
   from django import forms

   class ReservationForm(forms.Form):
       reservation_date = forms.DateField(widget=forms.SelectDateWidget)
   ```

   - `widget=forms.SelectDateWidget`: Provides a calendar interface for selecting dates.

4. **ChoiceField with Dropdown and Radio Buttons**

   ```python
   class FavoriteDishForm(forms.Form):
       DISH_CHOICES = [
           ('pasta', 'Pasta'),
           ('pizza', 'Pizza'),
           ('salad', 'Salad'),
       ]
       favorite_dish = forms.ChoiceField(choices=DISH_CHOICES, widget=forms.RadioSelect)
   ```

   - `choices`: Defines the options for the dropdown or radio buttons.
   - `widget=forms.RadioSelect`: Displays the choices as radio buttons instead of a dropdown.

### Conclusion

Using Django's `Form` class and various form fields, developers can create robust and user-friendly forms that collect and validate user input effectively. By leveraging the different field types and their parameters, you can ensure that the data collected is accurate and relevant to the application's requirements.

For more details and advanced usage, refer to the [Django documentation on forms](https://docs.djangoproject.com/en/stable/topics/forms/).

# Django fields

### Understanding Django Model Fields

In Django, a model is a Python class that represents a table in your database. The Django ORM (Object-Relational Mapping) translates this class into SQL queries to create, read, update, and delete data from the database. Each attribute of a model class corresponds to a database field.

#### Basic Model Definition

A typical Django model class subclasses `django.db.models.Model`. Here's an example of defining a simple model:

```python
from django.db import models  

class Person(models.Model): 
    first_name = models.CharField(max_length=20) 
    last_name = models.CharField(max_length=20)
```

When you run migrations, Django creates a corresponding table in the database:

```sql
CREATE TABLE myapp_person ( 
    id INTEGER PRIMARY KEY, 
    first_name VARCHAR(20), 
    last_name VARCHAR(20)  
);
```

#### Customizing Table Names

Django automatically names the table as `appname_modelname`, but you can override this by using the `db_table` parameter in the `Meta` class:

```python
class Student(models.Model): 
    # fields here...
    
    class Meta: 
        db_table = 'student_info'
```

#### Common Field Properties

- **primary_key**: Indicates if this field is the primary key for the table.
- **default**: Sets a default value or function for the field.
- **unique**: Ensures all values in this field are unique.
- **choices**: Provides a dropdown list of options for the field.

#### Field Types

Django's `models` module provides a variety of field types, each suitable for different kinds of data.

1. **CharField**: Stores string data, max length required.
   ```python
   first_name = models.CharField(max_length=50)
   ```

2. **IntegerField**: Stores integer values.
   ```python
   age = models.IntegerField()
   ```

3. **FloatField**: Stores floating-point numbers.
   ```python
   price = models.FloatField()
   ```

4. **DecimalField**: Stores decimal numbers with fixed precision.
   ```python
   grade = models.DecimalField(max_digits=5, decimal_places=2)
   ```

5. **DateTimeField**: Stores date and time.
   ```python
   created_at = models.DateTimeField(auto_now_add=True)
   ```

6. **EmailField**: CharField with email validation.
   ```python
   email = models.EmailField()
   ```

7. **FileField**: For file uploads.
   ```python
   file = models.FileField(upload_to='uploads/')
   ```

8. **ImageField**: FileField with image validation.
   ```python
   image = models.ImageField(upload_to='images/')
   ```

9. **URLField**: CharField with URL validation.
   ```python
   website = models.URLField()
   ```

#### Relationship Fields

1. **ForeignKey**: Establishes a one-to-many relationship.
   ```python
   class Customer(models.Model):
       name = models.CharField(max_length=255)

   class Vehicle(models.Model):
       name = models.CharField(max_length=255)
       customer = models.ForeignKey(Customer, on_delete=models.CASCADE)
   ```

2. **OneToOneField**: Creates a one-to-one relationship.
   ```python
   class College(models.Model):
       name = models.CharField(max_length=50)

   class Principal(models.Model):
       college = models.OneToOneField(College, on_delete=models.CASCADE)
   ```

3. **ManyToManyField**: Creates a many-to-many relationship.
   ```python
   class Teacher(models.Model):
       name = models.CharField(max_length=50)

   class Subject(models.Model):
       name = models.CharField(max_length=30)
       teachers = models.ManyToManyField(Teacher)
   ```

#### ForeignKey `on_delete` Options

- **CASCADE**: Deletes the object if the referenced object is deleted.
- **PROTECT**: Prevents deletion of the referenced object.
- **RESTRICT**: Raises `RestrictedError` on deletion unless part of a larger cascade.
- **SET_NULL**: Sets the field to NULL if the referenced object is deleted.
- **SET_DEFAULT**: Sets the field to its default value if the referenced object is deleted.
- **SET()**: Sets the field to the value returned by a callable.

### Example of ForeignKey Relationships

```python
class Artist(models.Model): 
    name = models.CharField(max_length=10)

class Album(models.Model): 
    artist = models.ForeignKey(Artist, on_delete=models.CASCADE)

class Song(models.Model): 
    artist = models.ForeignKey(Artist, on_delete=models.CASCADE) 
    album = models.ForeignKey(Album, on_delete=models.RESTRICT)
```

Creating instances and handling deletions:

```python
>>> artist1 = Artist.objects.create(name='Danny') 
>>> artist2 = Artist.objects.create(name='John') 
>>> album1 = Album.objects.create(artist=artist1) 
>>> album2 = Album.objects.create(artist=artist2) 
>>> song1 = Song.objects.create(artist=artist1, album=album1) 
>>> song2 = Song.objects.create(artist=artist1, album=album2)

# Deleting artist1 is safe, but deleting artist2 will raise RestrictedError due to the relationship with album2.
```

### Conclusion

Understanding Django's model fields and their properties is crucial for efficiently designing your database schema. By selecting the appropriate field types and properties, you can ensure data integrity and simplify data validation and handling. For more detailed information, refer to the [Django documentation](https://docs.djangoproject.com/en/stable/topics/db/models/).

# Form API

### Django Forms API

#### HTML Form

HTML forms are used to collect data from users. Here’s an example of a basic HTML form:

```html
<form action="/form/" method="POST"> 
    <label for="Name">Name of Applicant</label> 
    <input type="text" id="name" name="name"> 

    <label for="Address">Address</label> 
    <input type="text" id="add" name="add"> 

    <label for="Post">Post</label> 
    <select id="Post" name="Post"> 
      <option value="Manager">Manager</option>
      <option value="Cashier">Cashier</option>
      <option value="Operator">Operator</option>
    </select>
</form>
```

#### Django Form

Django’s `Form` class from the `django.forms` module simplifies form creation and handling. Here’s a basic example:

```python
from django import forms

class ApplicationForm(forms.Form):
    name = forms.CharField(label='Name of Applicant', max_length=50)
    address = forms.CharField(label='Address', max_length=100)
    posts = (('Manager', 'Manager'), ('Cashier', 'Cashier'), ('Operator', 'Operator'))
    post = forms.ChoiceField(choices=posts)
```

#### Form Fields

Common Django form fields and their HTML equivalents include:

- **CharField**: Translates to `<input type="text">`
- **IntegerField**: Only accepts integer numbers
- **FloatField**: Accepts float numbers
- **FileField**: `<input type="file">` for file uploads
- **ImageField**: Validates uploaded files as images
- **EmailField**: Validates email addresses
- **ChoiceField**: Corresponds to `<select>`

Here’s an example of defining these fields:

```python
from django import forms

class ExampleForm(forms.Form):
    name = forms.CharField(label="Enter first name", max_length=50)
    age = forms.IntegerField(min_value=20, max_value=60)
    price = forms.FloatField()
    upload = forms.FileField()
    email = forms.EmailField(max_length=254)
    gender = forms.ChoiceField(choices=[('M', 'Male'), ('F', 'Female')])
```

#### Rendering Forms

When you print a form instance, it generates HTML form elements:

```python
>>> from myapp.forms import ApplicationForm
>>> f = ApplicationForm()
>>> print(f)
```

This will output the corresponding HTML, like this:

```html
<tr>
    <th><label for="id_name">Name of Applicant:</label></th>
    <td>
        <input type="text" name="name" maxlength="50" required id="id_name">
    </td>
</tr>
<tr>
    <th><label for="id_address">Address:</label></th>
    <td>
        <input type="text" name="address" maxlength="100" required id="id_address">
    </td>
</tr>
```

#### Form Template

To render a form in a Django template, create an HTML file, such as `form.html`:

```html
<html>
<body>
    <form action="/form" method="POST">
        {% csrf_token %}
        <table>
            {{ form }}
        </table>
    </form>
</body>
</html>
```

In your view, pass the form to the template:

```python
from django.shortcuts import render
from .forms import ApplicationForm

def index(request):
    form = ApplicationForm()
    return render(request, 'form.html', {'form': form})
```

#### Rendering Forms in Different Formats

You can render forms in various formats using Django template tags:

- **As a table**: `{{ form.as_table }}`

```html
<table>
    <tr>
        <th><label for="id_name">Name of Applicant:</label></th>
        <td><input type="text" name="name" maxlength="50" required id="id_name"></td>
    </tr>
    <tr>
        <th><label for="id_address">Address:</label></th>
        <td><input type="text" name="address" maxlength="100" required id="id_address"></td>
    </tr>
</table>
```

- **As paragraphs**: `{{ form.as_p }}`

```html
<p>
    <label for="id_name">Name of Applicant:</label>
    <input type="text" name="name" maxlength="50" required id="id_name">
</p>
<p>
    <label for="id_address">Address:</label>
    <input type="text" name="address" maxlength="100" required id="id_address">
</p>
```

- **As unordered list**: `{{ form.as_ul }}`

```html
<li>
    <label for="id_name">Name of Applicant:</label>
    <input type="text" name="name" maxlength="50" required id="id_name">
</li>
<li>
    <label for="id_address">Address:</label>
    <input type="text" name="address" maxlength="100" required id="id_address">
</li>
```

- **As divs**: Manually create divs around each field.

```html
<div>
    <label for="id_name">Name of Applicant:</label>
    <input type="text" name="name" maxlength="50" required id="id_name">
</div>
<div>
    <label for="id_address">Address:</label>
    <input type="text" name="address" maxlength="100" required id="id_address">
</div>
```

#### Processing Form Data

To handle form submission, you need a view that processes the form data:

```python
from django.shortcuts import render
from .forms import ApplicationForm

def form_view(request):
    if request.method == 'POST':
        form = ApplicationForm(request.POST)
        if form.is_valid():
            name = form.cleaned_data['name']
            address = form.cleaned_data['address']
            post = form.cleaned_data['post']
            # Process the data
    else:
        form = ApplicationForm()
    return render(request, 'form.html', {'form': form})
```

#### Summary

Django’s `Form` class provides a powerful way to handle form creation, validation, and processing. By using the built-in fields and methods, you can easily render forms in templates and handle user input efficiently. This approach not only simplifies form handling but also ensures that your application remains scalable and maintainable.

# Creating Forms

In this video tutorial, you learned how to create a form using Django’s Form class, render it in a template, and utilize some of Django’s form features, such as validation. The example demonstrated involves creating a form for employees of the Little Lemon website to log their entry times.

#### Creating the Form

1. **Setup**:
   - Create a file called `forms.py` inside the app directory.

2. **Defining the Form**:
   - Import the `forms` module from Django: `from django import forms`.
   - Create a class `InputForm` that inherits from `forms.Form`.

3. **Adding Fields**:
   - **First Name and Last Name**: Add `CharField` attributes with a max length of 200.
   - **Shift**: Add a `ChoiceField` to allow selection between morning, afternoon, and evening shifts. Use a constant called `shifts` to define these choices.
   - **Time Log**: Add a `TimeField` to log the entry time for employees.

   ```python
   from django import forms

   class InputForm(forms.Form):
       first_name = forms.CharField(max_length=200)
       last_name = forms.CharField(max_length=200)
       SHIFTS = (
           ('Morning', 'Morning'),
           ('Afternoon', 'Afternoon'),
           ('Evening', 'Evening')
       )
       shift = forms.ChoiceField(choices=SHIFTS)
       time_log = forms.TimeField()
   ```

#### Rendering the Form

1. **Creating a View**:
   - Import the form in `views.py`.
   - Create a view function `form_view` to handle the form display and submission.

   ```python
   from django.shortcuts import render
   from .forms import InputForm

   def form_view(request):
       form = InputForm()
       context = {'form': form}
       return render(request, 'home.html', context)
   ```

2. **Creating the Template**:
   - Create a `templates` folder inside the app directory.
   - Inside `templates`, create `home.html`.
   - Include the form using the POST method and add a CSRF token for security.

   ```html
   <html>
   <body>
       <form action="" method="POST">
           {% csrf_token %}
           {{ form.as_p }}
           <input type="submit" value="Submit">
       </form>
   </body>
   </html>
   ```

#### Updating Configuration

1. **URLs Configuration**:
   - Update `urls.py` to include the path to the new view.

   ```python
   from django.urls import path
   from .views import form_view

   urlpatterns = [
       path('form/', form_view, name='form_view'),
   ]
   ```

2. **Settings Configuration**:
   - Ensure the app is referenced in `settings.py`.

#### Running the Server

- Use the command `python manage.py runserver` to start the development server.
- Navigate to the form URL to view and interact with the form.

#### Enhancing the Form

1. **Rendering with Paragraph Tags**:
   - Modify `home.html` to render form elements as paragraphs by using `{{ form.as_p }}`.
   - This organizes form elements one after another in paragraph tags.

2. **Form Validation**:
   - By default, all fields are required. To make a field optional, add `required=False` in the form definition.

   ```python
   first_name = forms.CharField(max_length=200, required=False)
   ```

3. **Help Text**:
   - Add `help_text` to provide additional guidance next to form fields.

   ```python
   time_log = forms.TimeField(help_text='Enter the exact time')
   ```

4. **Styling**:
   - Inline styling can be added in the template to customize the form’s appearance.

   ```html
   <form action="" method="POST" style="background-color: #f2f2f2;">
       {% csrf_token %}
       {{ form.as_p }}
       <input type="submit" value="Submit">
   </form>
   ```

#### Conclusion

In this video, you have learned how to:
- Create a form using Django’s Form class.
- Render the form in a template.
- Utilize Django’s form features such as validation and help text.

Although processing and saving the form data were not covered in this video, it hints that Django provides functionalities similar to form creation for processing and saving form data, which will be explored later in the course.

# Model Form
Django ModelForm

#### Introduction

In this tutorial, you learned how to create a `ModelForm` in Django, which allows you to save form data directly to the database. This approach integrates Django models and forms, providing a seamless way to handle form data and database entries.

#### Creating the ModelForm

1. **Setup**:
   - Ensure you have a model to bind with your form. The example used is a reservation form for a restaurant.

2. **Defining the Model**:
   - First, define the model in `models.py`.

   ```python
   from django.db import models

   class Logger(models.Model):
       first_name = models.CharField(max_length=200)
       last_name = models.CharField(max_length=200)
       SHIFTS = (
           ('Morning', 'Morning'),
           ('Afternoon', 'Afternoon'),
           ('Evening', 'Evening')
       )
       shift = models.CharField(choices=SHIFTS, max_length=10)
       time_log = models.TimeField()
   ```

3. **Creating the ModelForm**:
   - In `forms.py`, import the model and create a `ModelForm`.

   ```python
   from django import forms
   from .models import Logger

   class LogForm(forms.ModelForm):
       class Meta:
           model = Logger
           fields = '__all__'
   ```

#### Rendering the ModelForm

1. **Creating a View**:
   - Define a view function in `views.py` to handle the form display and submission.

   ```python
   from django.shortcuts import render, redirect
   from .forms import LogForm

   def form_view(request):
       if request.method == 'POST':
           form = LogForm(request.POST)
           if form.is_valid():
               form.save()
               return redirect('success_url')  # Redirect to a success page after saving
       else:
           form = LogForm()
       return render(request, 'home.html', {'form': form})
   ```

2. **Creating the Template**:
   - Create a `home.html` file in the `templates` folder to render the form.

   ```html
   <html>
   <body>
       <form action="" method="POST">
           {% csrf_token %}
           {{ form.as_p }}
           <input type="submit" value="Submit">
       </form>
   </body>
   </html>
   ```

#### Updating Configuration

1. **Registering the Model**:
   - Register the model in `admin.py` to manage it through the Django admin interface.

   ```python
   from django.contrib import admin
   from .models import Logger

   admin.site.register(Logger)
   ```

2. **URLs Configuration**:
   - Update `urls.py` to include the path to the new view.

   ```python
   from django.urls import path
   from .views import form_view

   urlpatterns = [
       path('form/', form_view, name='form_view'),
   ]
   ```

3. **Settings Configuration**:
   - Ensure the app is referenced in `settings.py`.

#### Running the Server

- Use the command `python manage.py runserver` to start the development server.
- Navigate to the form URL to view and interact with the form.

#### Processing and Saving Form Data

1. **Handling POST Requests**:
   - Inside the view function, check if the request method is POST.
   - Create a form instance with the POST data: `form = LogForm(request.POST)`.
   - Validate the form and save it if valid.

2. **Form Validation**:
   - Django’s form validation ensures that the data entered is correct before saving it to the database.

#### Running Migrations

- Run the command `python manage.py makemigrations` to create migrations for the model.
- Run `python manage.py migrate` to apply the migrations and create the database table.

#### Conclusion

In this tutorial, you learned how to:
- Create a `ModelForm` in Django.
- Bind the form to a model to save form data directly to the database.
- Render the form in a template.
- Handle form submissions and validate form data.

Using `ModelForm` simplifies the process of creating and managing forms and ensures data consistency and integrity by leveraging Django’s ORM capabilities. This method provides a streamlined approach to handling user inputs and storing them in a database, making development more efficient and secure.

# Exercise: Working with Forms
Goal
- Learner will create a ModelForm called BookingForm using the Booking model.

Objectives
- The Learner will practice creating a ModelForm using a model.
- The Learner will add form fields to the form as per the instructions.
- The Learner will populate a database table with data entered from a form on a webpage.

# Additional resources
The following resources will be helpful as additional references in dealing with different concepts related to the topics you have covered in this module.

Models – official documentation
- https://docs.djangoproject.com/en/4.1/topics/db/models/

Migrations – official documentation
- https://docs.djangoproject.com/en/4.1/topics/migrations/

Using Models – Mozilla
- https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Models

Detailed overview of Migrations
- https://docs.djangoproject.com/en/4.1/topics/migrations/#module-django.db.migrations

Migration operations:
- https://docs.djangoproject.com/en/4.1/ref/migration-operations/


