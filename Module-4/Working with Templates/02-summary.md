# Working with template language

### Working with Django Template Language (DTL)

Django Template Language (DTL) is a powerful feature of Django that separates presentation and application logic, allowing developers to create dynamic web pages efficiently. By using DTL, developers can embed dynamic content within static HTML, ensuring a clear separation of concerns and promoting the DRY (Don't Repeat Yourself) principle.

#### Key Constructs of DTL

DTL comprises four main constructs:
1. **Variables**
2. **Tags**
3. **Filters**
4. **Comments**

Let's delve into each of these constructs.

#### Variables

Variables in DTL are surrounded by double curly braces `{{ }}`. When the template engine encounters a variable, it evaluates the variable and replaces it with its value.

**Example**:
```html
<!-- about.html -->
<h1>Welcome to {{ restaurant_name }} restaurant</h1>
```

In your view, you can pass the variable `restaurant_name`:
```python
def about(request):
    context = {
        "restaurant_name": "Little Lemon"
    }
    return render(request, 'about.html', context)
```

This would render as:
```html
<h1>Welcome to Little Lemon restaurant</h1>
```

You can also access dictionary keys, object attributes, and list indices using dot notation.

#### Tags

Tags are used for adding logic to templates. They are enclosed within `{% %}`.

**Common Tags**:

1. **if tag**:
   ```html
   {% if logged_in %}
       <p>Welcome back!</p>
   {% else %}
       <a href="/login/">Log in</a>
   {% endif %}
   ```

2. **for tag**:
   ```html
   <ul>
       {% for item in menu %}
           <li>{{ item.name }} - ${{ item.price }}</li>
       {% endfor %}
   </ul>
   ```

In your view, you can pass a list of menu items:
```python
def menu(request):
    context = {
        "menu": [
            {"name": "Pasta", "price": 12},
            {"name": "Pizza", "price": 10},
            {"name": "Salad", "price": 8}
        ]
    }
    return render(request, 'menu.html', context)
```

This would render as:
```html
<ul>
    <li>Pasta - $12</li>
    <li>Pizza - $10</li>
    <li>Salad - $8</li>
</ul>
```

3. **extends and include**:
   - **extends** is used for template inheritance.
   - **include** is used to include other templates.

#### Filters

Filters modify the value of variables and are applied using the pipe `|` symbol.

**Example**:
```html
{{ name|upper }}
```
If `name` is "john", it will render as "JOHN".

Filters can also take arguments:
```html
{{ date|date:"Y-m-d" }}
```
If `date` is a datetime object, it will render as "2023-06-20".

**Common Filters**:
- `upper`: Converts a string to uppercase.
- `lower`: Converts a string to lowercase.
- `title`: Capitalizes the first letter of each word.
- `length`: Returns the number of items in a list.

#### Comments

Comments are ignored by Django and are used for documentation. They are enclosed within `{# #}`.

**Example**:
```html
{# This is a comment and will not be rendered #}
```

### Practical Example: Creating a Dynamic About Page

Let's create an "About" page for the Little Lemon website.

#### Step-by-Step Implementation

1. **View Function**:
   ```python
   from django.shortcuts import render

   def about(request):
       about_content = {
           "about": "Little Lemon is a charming bistro serving fresh and delicious meals."
       }
       return render(request, 'about.html', about_content)
   ```

2. **URL Configuration**:
   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('about/', views.about, name='about'),
   ]
   ```

3. **Settings Configuration**:
   Ensure your settings are correct:
   ```python
   import os

   TEMPLATES = [
       {
           'BACKEND': 'django.template.backends.django.DjangoTemplates',
           'DIRS': [os.path.join(BASE_DIR, 'templates')],
           'APP_DIRS': True,
           'OPTIONS': {
               'context_processors': [
                   'django.template.context_processors.debug',
                   'django.template.context_processors.request',
                   'django.contrib.auth.context_processors.auth',
                   'django.contrib.messages.context_processors.messages',
               ],
           },
       },
   ]

   INSTALLED_APPS = [
       ...
       'your_app_name',
       ...
   ]
   ```

4. **Template File**:
   Create `about.html` in the `templates` directory:
   ```html
   <!-- about.html -->
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>About Little Lemon</title>
       <style>
           body {
               background-color: #f0f8ff;
               font-family: Arial, sans-serif;
           }
       </style>
   </head>
   <body>
       <h2>About</h2>
       <p>{{ about }}</p>
   </body>
   </html>
   ```

5. **Testing**:
   Run the server and navigate to `http://localhost:8000/about/` to see the dynamic content.

### Summary

By understanding and utilizing Django Template Language, developers can efficiently manage dynamic content in their web applications. The use of variables, tags, filters, and comments in templates ensures a clean separation of presentation and logic, making the development process more organized and maintainable.

# Template Language and Variable Interpolation

### Understanding Django Template Language (DTL)

In modern web frameworks, a template engine merges data sources with static HTML to generate dynamic web pages. Django uses its own template engine, Django Template Language (DTL), which integrates seamlessly with Django's views and models. This reading explores DTL's core components: variables, tags, and filters, and how they contribute to dynamic content rendering.

#### Template Engine Overview

A web template is a static HTML document interspersed with placeholders of template language code. The template engine processes this document, substituting placeholders with actual data to produce the final HTML sent to the client browser.

**Diagram: Template Engine Workflow**
```
Data Source (e.g., Database) --> Template Engine --> Generated HTML Content --> Browser
```

![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/rQ9T1S93R0uJLLoHGXBGzw_6751273228694b20ac0cfec46ea835e1_C5M4L2_Itm02_1.png?expiry=1719014400000&hmac=fhgoRuRvjy98dR8INRRpgGTUxNS_GjBhUu6KcsmxPuY)

#### Components of DTL

DTL consists of three main components:
1. **Variables**
2. **Tags**
3. **Filters**

### Template Variables

Variables in DTL are used to dynamically insert data into templates. They are surrounded by double curly braces `{{ }}`.

**Example: Basic Variable Usage**
```html
<!-- index.html -->
<h2>Welcome, {{ name }}</h2>
```

In the view, you pass the variable `name`:
```python
def index(request, name):       
    context = {'name': name}   
    return render(request, 'index.html', context)
```
If the URL is `http://localhost:8000/myapp/Novak`, the output will be:
```html
<h2>Welcome, Novak</h2>
```

Variables can also access dictionary keys, object attributes, and list indices using dot notation.

**Example: Dictionary Variable**
```python
person = {'name': 'Roger', 'profession': 'Teacher'}
return render(request, 'person.html', {'person': person})
```
In the template:
```html
Name: {{ person.name }} <br>
Profession: {{ person.profession }}
```
Output:
```
Name: Roger
Profession: Teacher
```

### Template Tags

Tags in DTL add logic to templates. They are enclosed within `{% %}` symbols and function similarly to programming language keywords.

#### If Tag

The `if` tag evaluates a condition and renders content conditionally.

**Example: Conditional Rendering**
```python
def index(request): 
    context = {'user': 'admin'} 
    return render(request, 'index.html', context)
```
In the template:
```html
{% if user == "admin" %} 
    <h2>Welcome, {{ user }}</h2> 
{% else %} 
    <h2>Welcome, Guest. You don't have admin access.</h2> 
{% endif %}
```

#### For Tag

The `for` tag iterates over a sequence (e.g., list, tuple, dictionary) and renders content for each item.

**Example: List Iteration**
```python
def myview(request): 
    langs = ['Python', 'Java', 'PHP', 'Ruby', 'Rust'] 
    return render(request, 'langs.html', {'langs': langs})
```
In the template:
```html
<h1>List of Languages</h1>
<ul> 
    {% for lang in langs %} 
        <li>{{ lang }}</li> 
    {% endfor %} 
</ul>
```
Output:
```
List of Languages
- Python
- Java
- PHP
- Ruby
- Rust
```

DTL also provides helper variables within loops:
- `forloop.counter`: Current iteration (1-indexed).
- `forloop.revcounter`: Iteration from the end (1-indexed).
- `forloop.first`: True if this is the first iteration.
- `forloop.last`: True if this is the last iteration.
- `forloop.parentloop`: Reference to the parent loop in nested loops.

**Example: Using Helper Variables**
```html
<ul> 
    {% for lang in langs %} 
        <li>{{ forloop.counter }}: {{ lang }}</li> 
    {% endfor %} 
</ul>
```
Output:
```
1: Python
2: Java
3: PHP
4: Ruby
5: Rust
```

#### Other Tags

- **{% comment %}**: Comments out code within `{% comment %}` and `{% endcomment %}`.
- **{% block %}**: Defines a block that can be overridden by child templates (used in template inheritance).
- **{% csrf_token %}**: Includes a CSRF token for form security.
- **{% include %}**: Includes another template within the current template.
- **{% with %}**: Sets a local variable available within `{% with %}` and `{% endwith %}`.

### Filters

Filters modify the representation of variables. They are applied using the pipe `|` symbol.

**Common Filters**:
- **default**: Provides a default value if the variable is undefined.
- **join**: Joins list items with a specified separator.
- **length**: Returns the length of a sequence.
- **first**: Returns the first item of a list.
- **last**: Returns the last item of a list.
- **lower**: Converts a string to lowercase.
- **upper**: Converts a string to uppercase.
- **title**: Converts a string to title case.
- **wordcount**: Returns the number of words in a string.

**Example: Using Filters**
```html
<p>{{ name|upper }}</p> <!-- If name is "john", output: "JOHN" -->
<p>{{ list|first }}</p> <!-- If list is ['a', 'b', 'c'], output: 'a' -->
<p>{{ string|length }}</p> <!-- If string is "Django Template", output: 15 -->
```

### Practical Example: Dynamic About Page

Let's create a dynamic "About" page for the Little Lemon website.

**View Function**:
```python
def about(request):
    about_content = {"about": "Little Lemon is a charming bistro serving fresh and delicious meals."}
    return render(request, 'about.html', about_content)
```

**URL Configuration**:
```python
from django.urls import path
from . import views

urlpatterns = [
    path('about/', views.about, name='about'),
]
```

**Template File**:
```html
<!-- about.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>About Little Lemon</title>
    <style>
        body { background-color: #f0f8ff; font-family: Arial, sans-serif; }
    </style>
</head>
<body>
    <h2>About</h2>
    <p>{{ about }}</p>
</body>
</html>
```

### Summary

DTL is a powerful tool for dynamically generating web pages in Django. By mastering variables, tags, and filters, developers can efficiently manage the presentation logic of their applications, ensuring clean and maintainable code.

# Dynamic Templates in Django

## Using Templates to Display Dynamic Data in Django

In this video, you'll learn how to use Django templates to return a web page with dynamic data. We'll create templates, pass data from a dictionary to these templates, and display it on a web page. Additionally, we'll explore using a for loop to handle dictionary data inside a template.

### Step 1: Setting Up the View

1. **Create a View Function**:
   - Open your app's `views.py` file.
   - Define a view function that takes the request object as a parameter.

    ```python
    from django.shortcuts import render

    def menu(request):
        menu_item = {
            'name': 'Greek Salad'
        }
        return render(request, 'menu.html', menu_item)
    ```

2. **Dictionary Setup**:
   - Inside the function, create a dictionary.
   - Define `name` as the key and give it the value `Greek Salad`.
   - Assign the dictionary to a variable named `menu_item`.
   - Instead of returning an HTTP response, use the `render` function to pass the dictionary to the template.

### Step 2: Creating the Template

1. **Create a Template File**:
   - In your project directory, create a new folder called `templates`.
   - Inside the `templates` folder, create a new file named `menu.html`.

2. **Write Template Code**:
   - Open `menu.html` and use double curly braces to dynamically display the dictionary object.

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Menu</title>
    </head>
    <body>
        <h2>Welcome to Little Lemon's Menu</h2>
        <p>Today's special: {{ name }}</p>
    </body>
    </html>
    ```

### Step 3: Configuring Settings

1. **Update Settings**:
   - Open the `settings.py` file.
   - Ensure that the `TEMPLATES` DIRS attribute points to the templates folder.
   - Make sure the app is listed in the `INSTALLED_APPS`.

    ```python
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [os.path.join(BASE_DIR, 'templates')],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]
    ```

2. **URL Configuration**:
   - Open the `urls.py` file.
   - Inside the `urlpatterns` list, add the path function to map the URL to the view function.

    ```python
    from django.urls import path
    from . import views

    urlpatterns = [
        path('menu/', views.menu, name='menu'),
    ]
    ```

### Step 4: Running the Server

1. **Run the Server**:
   - Start the Django server using `python manage.py runserver`.
   - Open the browser and navigate to `http://localhost:8000/menu/`.
   - You should see the value stored in the dictionary displayed in the browser.

### Step 5: Updating the View for More Complex Data

1. **More Complex Data**:
   - Update the `menu` view function to include more values in the dictionary.

    ```python
    def menu(request):
        new_menu = {
            'mains': [
                {'name': 'Greek Salad', 'price': 10},
                {'name': 'Bruschetta', 'price': 8},
                {'name': 'Grilled Fish', 'price': 15}
            ]
        }
        return render(request, 'menu.html', new_menu)
    ```

2. **Update Template for Looping**:
   - Modify the `menu.html` to loop over each item in the dictionary using a for loop.

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Menu</title>
    </head>
    <body>
        <h2>Welcome to Little Lemon's Menu</h2>
        <table>
            <tr>
                <th>Name</th>
                <th>Price</th>
            </tr>
            {% for item in mains %}
            <tr>
                <td>{{ item.name }}</td>
                <td>{{ item.price }}</td>
            </tr>
            {% endfor %}
        </table>
    </body>
    </html>
    ```

3. **Add More Items**:
   - Add another item to the `new_menu` dictionary.

    ```python
    def menu(request):
        new_menu = {
            'mains': [
                {'name': 'Greek Salad', 'price': 10},
                {'name': 'Bruschetta', 'price': 8},
                {'name': 'Grilled Fish', 'price': 15},
                {'name': 'Pasta', 'price': 12}
            ]
        }
        return render(request, 'menu.html', new_menu)
    ```

4. **Refresh the Page**:
   - Save the file and refresh the page to see the dynamically updated values.

### Conclusion

In this video, you learned how to return a web page with dynamic data using Django templates. We explored how to create templates, pass data from a dictionary, and display it on a web page using a for loop. This method allows you to create dynamic and interactive web pages by leveraging Django's powerful template system.

# Mapping model objects to a template

## Creating a Dynamic Menu in Django Using Model Objects

In this video, you'll learn how to create a dynamic menu for "Little Lemon" by passing a model object to a template from a view. We'll also explore looping over an object and displaying its content using a for-loop with Django's template language.

### Step 1: Setting Up the Model

1. **Define the Model**:
   - Open the `models.py` file and ensure you have a model named `Menu` with attributes for `name` and `price`.

    ```python
    from django.db import models

    class Menu(models.Model):
        name = models.CharField(max_length=100)
        price = models.IntegerField()

        def __str__(self):
            return self.name
    ```

   - Ensure your model is registered in the Django admin interface and that you have some data entered for menu items like hummus, falafel, etc.

### Step 2: Creating the View Function

1. **Define the View**:
   - Open the `views.py` file.
   - Create a view function named `menu_by_id`.

    ```python
    from django.shortcuts import render
    from .models import Menu

    def menu_by_id(request):
        new_menu = Menu.objects.all()
        new_menu_dict = {'menu': new_menu}
        return render(request, 'menu_card.html', new_menu_dict)
    ```

2. **Handle Previous Code**:
   - Comment out any previous view functions to avoid conflicts.

### Step 3: Configuring URLs

1. **Update URL Configuration**:
   - Open the `urls.py` file.
   - Add a new path for the `menu_by_id` view.

    ```python
    from django.urls import path
    from . import views

    urlpatterns = [
        path('menu_card/', views.menu_by_id, name='menu_by_id'),
    ]
    ```

### Step 4: Creating the Template

1. **Create Template File**:
   - Inside the `templates` directory, create a file named `menu_card.html`.

2. **Write Template Code**:
   - Use Django template language to loop through the `menu` object and display each item.

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <title>Menu Card</title>
    </head>
    <body>
        <h2>Menu</h2>
        {% if menu %}
            {% for item in menu %}
                <p>{{ item.name }} - ${{ item.price }}</p>
            {% endfor %}
        {% else %}
            <p>No menu items available.</p>
        {% endif %}
    </body>
    </html>
    ```

### Step 5: Running the Server

1. **Run the Server**:
   - Start the Django server using `python manage.py runserver`.
   - Open the browser and navigate to `http://localhost:8000/menu_card/`.
   - You should see the menu items displayed dynamically.

### Step 6: Adding New Items

1. **Add New Items via Admin**:
   - Navigate to the Django admin interface.
   - Add a new menu item, e.g., Baklava with a price of 10.
   - Save the item and refresh the menu page to see the updates automatically.

### Conclusion

In this video, you learned how to create a dynamic menu by passing a model object to a template from a view. We explored how to loop over an object and display its content using a for-loop with Django's template language. This approach allows you to quickly and dynamically render data stored in a database onto a web page, enhancing the interactivity and dynamism of your website.

# Exercise: Creating Dynamic Templates

### **Goal**

- The learner will practice using Dynamic Templates in Django

### **Objectives**

- The learner will fetch dynamic content from the model inside the view
- The learner will update view created for Menu with dynamic content
- The learner will pass the dynamic content to the template and run a loop to fetch item values.


# Template inheritance

## Using Template Inheritance in Django to Create a Dynamic Web Layout

### Introduction
When developing web applications, it’s essential to adhere to the DRY (Don't Repeat Yourself) principle to avoid duplication and ensure maintainability. Django's template inheritance system allows developers to create reusable components like headers and footers, ensuring consistency and reducing redundancy across multiple pages.

In this guide, we'll learn how to create a base template for the Little Lemon website and use template inheritance to include common elements like headers and footers across different pages.

### Step 1: Creating the Base Template

1. **Base Template**:
   - Create a new file named `base.html` inside your `templates` directory.
   - Define the common layout for your website including the header and footer sections.

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>{% block title %}Little Lemon{% endblock %}</title>
        <link rel="stylesheet" href="{% static 'css/styles.css' %}">
    </head>
    <body>
        <header>
            <!-- Header content goes here -->
            <h1>Little Lemon</h1>
            <nav>
                <ul>
                    <li><a href="{% url 'home' %}">Home</a></li>
                    <li><a href="{% url 'about' %}">About</a></li>
                    <li><a href="{% url 'menu' %}">Menu</a></li>
                    <li><a href="{% url 'book' %}">Book</a></li>
                </ul>
            </nav>
        </header>

        <main>
            {% block content %}
            {% endblock %}
        </main>

        <footer>
            <!-- Footer content goes here -->
            <p>&copy; 2024 Little Lemon. All rights reserved.</p>
        </footer>
    </body>
    </html>
    ```

### Step 2: Creating Specific Page Templates

1. **About Page Template**:
   - Create a file named `about.html` in your `templates` directory.
   - Extend the `base.html` template and define the content block.

    ```html
    {% extends 'base.html' %}

    {% block title %}About Us - Little Lemon{% endblock %}

    {% block content %}
    <h2>About Us</h2>
    <p>Welcome to Little Lemon. We are a family-run restaurant offering the finest Mediterranean cuisine.</p>
    {% endblock %}
    ```

2. **Menu Page Template**:
   - Create a file named `menu.html` in your `templates` directory.

    ```html
    {% extends 'base.html' %}

    {% block title %}Menu - Little Lemon{% endblock %}

    {% block content %}
    <h2>Our Menu</h2>
    <!-- Dynamic content for the menu items will be inserted here -->
    {% if menu %}
        <ul>
            {% for item in menu %}
                <li>{{ item.name }} - ${{ item.price }}</li>
            {% endfor %}
        </ul>
    {% else %}
        <p>No menu items available.</p>
    {% endif %}
    {% endblock %}
    ```

### Step 3: Updating Views to Use Templates

1. **View for About Page**:
   - In your `views.py` file, create a view for the about page.

    ```python
    from django.shortcuts import render

    def about(request):
        return render(request, 'about.html')
    ```

2. **View for Menu Page**:
   - In your `views.py` file, create a view for the menu page.

    ```python
    from django.shortcuts import render
    from .models import Menu

    def menu(request):
        menu_items = Menu.objects.all()
        return render(request, 'menu.html', {'menu': menu_items})
    ```

### Step 4: Configuring URLs

1. **Update URL Configuration**:
   - Open the `urls.py` file.
   - Add paths for the about and menu views.

    ```python
    from django.urls import path
    from . import views

    urlpatterns = [
        path('about/', views.about, name='about'),
        path('menu/', views.menu, name='menu'),
        path('home/', views.home, name='home'),  # Assuming a home view exists
        path('book/', views.book, name='book'),  # Assuming a book view exists
    ]
    ```

### Conclusion

By using Django's template inheritance, you can efficiently manage and update common elements across multiple pages, ensuring a consistent look and feel for your website. This approach adheres to the DRY principle, making your code more maintainable and easier to update. With the base template in place, adding new pages to your site becomes much simpler, as you only need to focus on the unique content for each page.

# More on Template inheritance

### Template Inheritance in Django

Template inheritance in Django allows for the reuse of common components across multiple web pages, ensuring consistency and maintainability. It is similar to inheritance in object-oriented programming, where child classes inherit properties from a parent class.

#### Basic Structure of Template Inheritance

1. **Base Template (`base.html`)**:
   - This is the parent template that includes common elements such as headers, footers, and navigation bars. These elements are placed within `{% block %}` tags, allowing child templates to override them as needed.

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>{% block title %}Django Web Application{% endblock %}</title>
       <link rel="stylesheet" href="{% static 'css/styles.css' %}">
   </head>
   <body>
       <!-- Header -->
       <header>
           <h2 align="center">Django Web Application</h2>
           <hr>
       </header>
       
       <!-- Sidebar and Content -->
       <div style="display: flex;">
           <!-- Sidebar -->
           <div style="width: 20%; border-right: groove;">
               <ul>
                   <li><a href="{% url 'home' %}">Home</a></li>
                   <li><a href="{% url 'register' %}">Register</a></li>
                   <li><a href="{% url 'login' %}">Login</a></li>
               </ul>
           </div>
           
           <!-- Main Content -->
           <div style="width: 80%; padding-left: 20px;">
               {% block content %}
               {% endblock %}
           </div>
       </div>
       
       <!-- Footer -->
       <footer>
           <hr>
           {% block footer %}
           <h4 align="right">All rights reserved</h4>
           {% endblock %}
       </footer>
   </body>
   </html>
   ```

2. **Child Templates**:
   - Child templates extend the base template and override specific blocks to insert page-specific content.

   - **Home Template (`home.html`)**:

     ```html
     {% extends "base.html" %}

     {% block title %}Home - Django Web Application{% endblock %}

     {% block content %}
     <h2 align="center">This is Home page</h2>
     {% endblock %}
     ```

   - **Register Template (`register.html`)**:

     ```html
     {% extends "base.html" %}

     {% block title %}Register - Django Web Application{% endblock %}

     {% block content %}
     <h2 align="center">Registration Form appears here</h2>
     {% endblock %}
     ```

   - **Login Template (`login.html`)**:

     ```html
     {% extends "base.html" %}

     {% block title %}Login - Django Web Application{% endblock %}

     {% block content %}
     <h2 align="center">Login Form appears here</h2>
     {% endblock %}

     {% block footer %}
     {{ block.super }}
     <h4 align="right">Designed By: Alexa Designs Ltd</h4>
     {% endblock %}
     ```

#### Implementing Views

1. **Views (`views.py`)**:
   - Define view functions for each page that render the respective templates.

     ```python
     from django.shortcuts import render

     def home(request):
         return render(request, 'home.html')

     def register(request):
         return render(request, 'register.html')

     def login(request):
         return render(request, 'login.html')
     ```

2. **URL Configuration (`urls.py`)**:
   - Map URLs to the view functions.

     ```python
     from django.urls import path
     from . import views

     urlpatterns = [
         path('home/', views.home, name='home'),
         path('register/', views.register, name='register'),
         path('login/', views.login, name='login'),
     ]
     ```

#### Serving Static Files

1. **Static Files**:
   - Static files such as CSS, JavaScript, and images are served using Django’s static files mechanism.
   - Ensure `django.contrib.staticfiles` is included in `INSTALLED_APPS`.

2. **Static Files Configuration (`settings.py`)**:
   - Define the URL for static files.

     ```python
     STATIC_URL = '/static/'
     ```

3. **Loading Static Files in Templates**:
   - Use the `{% load static %}` tag to load static files.

     ```html
     {% load static %}
     <link rel="stylesheet" href="{% static 'css/styles.css' %}">
     ```

4. **Serving Images**:
   - Example of rendering an image from the static folder.

     ```html
     {% load static %}
     <img src="{% static 'my_app/example.jpg' %}" alt="Example Image">
     ```

### Conclusion

Template inheritance in Django is a powerful feature that promotes code reuse and maintainability. By defining a base template with common elements and using child templates to override specific blocks, developers can ensure a consistent layout across the application. Additionally, Django's mechanism for serving static files simplifies the inclusion of assets like stylesheets and images, further enhancing the development process.

# Working with Template inheritance

### Template Inheritance and Including Templates in Django

Template inheritance and the `include` tag are powerful tools in Django that help you create consistent and maintainable web pages. Let's walk through a step-by-step demonstration of using these features to build the Little Lemon website with three pages: Home, Menu, and About.

#### Step 1: Setting Up Views

First, create view functions for the Home, About, and Menu pages. Add the following code to your `views.py` file:

```python
from django.shortcuts import render

def home(request):
    return render(request, 'index.html')

def about(request):
    return render(request, 'about.html')

def menu(request):
    return render(request, 'menu.html')
```

#### Step 2: Configuring URLs

Update your URL configuration to map these views to their respective URLs. Update both the project-level and app-level `urls.py` files.

**Project-level `urls.py`:**

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('yourapp.urls')),  # Replace 'yourapp' with your app name
]
```

**App-level `urls.py`:**

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.home, name='home'),
    path('about/', views.about, name='about'),
    path('menu/', views.menu, name='menu'),
]
```

#### Step 3: Update `settings.py`

Ensure your app is included in the `INSTALLED_APPS` and that Django knows where to find your templates.

```python
INSTALLED_APPS = [
    # Other apps
    'yourapp',  # Add your app name here
]

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

#### Step 4: Setting Up the Template Directory

Create the following directory structure in your project:

```
myproject/
    templates/
        base.html
        index.html
        about.html
        menu.html
        partials/
            _header.html
```

#### Step 5: Creating `base.html`

Create `base.html` inside the `templates` folder. This will be your base template:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Little Lemon</title>
    <link rel="stylesheet" href="{% static 'css/styles.css' %}">
</head>
<body style="background-color: #f0f0f0;">
    {% include 'partials/_header.html' %}
    <main>
        {% block content %}
        {% endblock %}
    </main>
</body>
</html>
```

#### Step 6: Creating the Header Partial

Create `_header.html` inside the `partials` folder:

```html
<header>
    <h1>Welcome to Little Lemon</h1>
    <nav>
        <ul>
            <li><a href="{% url 'home' %}">Home</a></li>
            <li><a href="{% url 'about' %}">About</a></li>
            <li><a href="{% url 'menu' %}">Menu</a></li>
        </ul>
    </nav>
</header>
```

#### Step 7: Creating Child Templates

**`index.html`:**

```html
{% extends 'base.html' %}

{% block content %}
    <h2>Home Page</h2>
    <p>Welcome to the Little Lemon Home Page.</p>
{% endblock %}
```

**`about.html`:**

```html
{% extends 'base.html' %}

{% block content %}
    <h2>About Us</h2>
    <p>Learn more about Little Lemon.</p>
{% endblock %}
```

**`menu.html`:**

```html
{% extends 'base.html' %}

{% block content %}
    <h2>Our Menu</h2>
    <p>Explore the delicious menu of Little Lemon.</p>
{% endblock %}
```

#### Step 8: Running the Development Server

Run the development server to see the changes:

```bash
python3 manage.py runserver
```

Visit the following URLs to see the respective pages:
- Home: `http://localhost:8000/`
- About: `http://localhost:8000/about/`
- Menu: `http://localhost:8000/menu/`

### Summary

By using the `extends` and `include` tags, you can efficiently manage common elements across multiple pages in Django. The `base.html` template provides a consistent structure, and child templates override specific blocks to insert unique content. The `include` tag is used to embed reusable components like headers, ensuring a consistent layout without duplicating code. This approach simplifies maintenance and enhances the scalability of your web application.

# Exercise: Working with Templates
### **Goal**

- The learner will practice using partial Templates in Django

### **Objectives**

- The learner will use partial templates using keywords include and extends


# 

