# Templates

### Using Templates in Django

In Django, templates are a powerful feature for generating dynamic content on web pages. This dynamic content can change based on user behavior, context, and preferences. Here's a comprehensive overview of using templates in Django, including the Django Template Engine and the Django Template Language (DTL).

#### 1. Introduction to Templates

- **Templates**: Text-based documents or Python strings marked up using the Django Template Language (DTL). They consist of:
  - **Static Content**: The HTML that remains constant.
  - **Dynamic Content**: Content inserted using template language syntax.

#### 2. Creating Dynamic HTML Content

Example of using dynamic content in a template:

```html
<h1>{{ dish_name }}</h1>
<p>Price: ${{ price }}</p>
```

- **Static Part**: The HTML tags (`<h1>`, `<p>`).
- **Dynamic Part**: Variables within double curly braces (`{{ dish_name }}`, `{{ price }}`).

#### 3. Rendering Templates with Dynamic Content

The `render` function in Django handles rendering templates. It takes three parameters:
- **Request**: The HTTP request object.
- **Path**: The path to the template.
- **Dictionary of Variables**: A dictionary containing the context variables.

Example:

```python
from django.shortcuts import render

def menu_view(request):
    context = {'dish_name': 'Spaghetti Carbonara', 'price': 12.50}
    return render(request, 'menu.html', context)
```

#### 4. Django Template Engine and Language

- **Django Template Engine**: Bridges the gap between Python (dynamic) and HTML (static).
- **Django Template Language (DTL)**: Allows embedding dynamic content within templates. DTL includes:
  - **Variables**: Insert dynamic content (`{{ variable_name }}`).
  - **Tags**: Add logic to templates (`{% tag_name %}...{% endtag_name %}`).
  - **Filters**: Modify variables (`{{ variable_name|filter_name }}`).
  - **Comments**: Add comments (`{# This is a comment #}`).

#### 5. Template Configuration

In `settings.py`, configure the template engine settings:

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

- **APP_DIRS**: Tells Django to look for templates in a subdirectory named `templates` for each application.
- **DIRS**: Additional directories to search for templates.

#### 6. Extending Template Functionality

Django supports multiple template engines. You can use Jinja2 as an alternative to the default Django template engine:

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.jinja2.Jinja2',
        'DIRS': [os.path.join(BASE_DIR, 'jinja2_templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'environment': 'myapp.jinja2.Environment',
        },
    },
]
```

#### 7. Template Inheritance for Code Reuse

**Template Inheritance** allows creating a base template that child templates can extend. This promotes code reuse and follows the DRY (Don't Repeat Yourself) principle.

Example:

- **Base Template (base.html)**:

```html
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}My Site{% endblock %}</title>
</head>
<body>
    {% block content %}{% endblock %}
</body>
</html>
```

- **Child Template (home.html)**:

```html
{% extends "base.html" %}

{% block title %}Home Page{% endblock %}

{% block content %}
    <h1>Welcome to the Home Page</h1>
{% endblock %}
```

#### Conclusion

In this section, you learned about:
- Creating and using templates in Django.
- Understanding the Django Template Engine and Django Template Language.
- Configuring template settings in Django.
- Using template inheritance for code reuse.

Templates in Django are a fundamental aspect of generating dynamic content, and mastering them is essential for efficient web development. Continue exploring more advanced features and best practices to leverage the full potential of Django templates.

# Template Examples

### Template Examples in Django

Django's templating system allows developers to generate dynamic HTML content efficiently. In this reading, we will explore simple examples of using templates, variables, and filters in Django.

#### HTML Response with Django Views

Django's view function typically returns an `HttpResponse` with a content-type of "text/html". For instance, the following view function returns a simple HTML string:

```python
from django.http import HttpResponse 

def index(request): 
    return HttpResponse("<h1>Hello, world. </h1>")
```

Assuming the function is properly configured and mapped in the `urls.py` file, visiting the corresponding URL will display "Hello, world" within an `<h1>` tag.

#### Using Variables in Views

To make the HTML content dynamic, you can use Python’s string substitution:

```python
from django.http import HttpResponse 

def index(request, name): 
    return HttpResponse("<h1>Hello, {}. </h1>".format(name))
```

However, generating HTML directly in views can become cumbersome, especially with complex logic. This is where Django templates come into play.

#### Introduction to Django Templates

A template is an HTML file with placeholders for dynamic data marked by special syntax. Django uses its own template language, DTL (Django Template Language). Let's create a simple template file `hello.html`:

```html
<!-- hello.html -->
<html> 
<head> 
    <title>My Django website</title> 
</head> 
<body> 
    <h1>Hello, World</h1> 
</body> 
</html>
```

#### Rendering a Template

In the view function, you can render this template using Django's template system:

```python
from django.http import HttpResponse 
from django.template import loader 

def index(request): 
    template = loader.get_template('hello.html') 
    context = {} 
    return HttpResponse(template.render(context, request))
```

For simplicity, Django provides a shortcut method `render()`:

```python
from django.shortcuts import render 

def index(request): 
    return render(request, 'hello.html', {})
```

#### Passing Variables to Templates

You can pass variables to the template through the context dictionary:

```python
from django.shortcuts import render  

def index(request, name):  
    context = {"name": name}  
    return render(request, 'hello.html', context)
```

Modify `hello.html` to use the variable `name`:

```html
<!-- hello.html -->
<html>  
<body>  
    <h1>Hello, {{ name }}</h1>  
</body>  
</html>
```

#### Using Template Tags and Filters

Django's template language provides tags and filters to add logic and modify content within templates.

##### Conditional Logic with Tags

You can use the `{% if %}` tag for conditional logic:

```html
<!-- hello.html -->
<html>  
<body>  
    <h1>Hello, {{ name }}</h1>
    {% if name == "John" %}
        <p>Welcome back, John!</p>
    {% endif %}
</body>  
</html>
```

##### Looping with Tags

The `{% for %}` tag allows for looping through items:

```html
<!-- items.html -->
<html>
<body>
    <ul>
    {% for item in items %}
        <li>{{ item }}</li>
    {% endfor %}
    </ul>
</body>
</html>
```

##### Using Filters

Filters modify the value of variables. They are applied using the `|` symbol:

```html
<!-- hello.html -->
<html>  
<body>  
    <h1>Hello, {{ name | upper }}</h1>  
</body>  
</html>
```

Common filters include:
- **`upper`**: Converts to uppercase.
- **`lower`**: Converts to lowercase.
- **`title`**: Capitalizes the first letter of each word.
- **`length`**: Returns the number of items in a list.
- **`wordcount`**: Counts the words in a string.
- **`slice`**: Extracts a portion of a list.

Example with a list and the `length` filter:

```python
def index(request):  
    nums = [1, 2, 3, 4]  
    context = {"nums": nums}  
    return render(request, 'nums.html', context)
```

```html
<!-- nums.html -->
<html>  
<body>  
    <p>The list has {{ nums | length }} items.</p>  
</body>  
</html>
```

Example with `slice` filter:

```html
<!-- nums.html -->
<html>  
<body>  
    <p>Items: {{ nums | slice:":2" }}</p>  
</body>  
</html>
```

### Summary

- **Templates**: HTML files with placeholders for dynamic data.
- **View Function**: Uses `render()` to combine templates with context data.
- **Variables**: Passed to templates via the context dictionary.
- **Tags and Filters**: Provide logic and modify content within templates.

These features of Django's templating system make it powerful and flexible for generating dynamic web pages. In subsequent lessons, you will delve deeper into more advanced template tags and filters.

# Creating Templates

### Creating a Template in Django

Templates in Django are powerful tools for rendering dynamic content alongside static HTML. They promote code reusability and make it easier to manage dynamic webpages. In this guide, we'll explore how to create and use templates in Django by following a practical example.

#### Steps to Create a Template in Django

1. **Define a View Function**: Create a view function that returns the render function.
2. **Update URL Configurations**: Add the view to your URL patterns.
3. **Configure Settings**: Ensure the settings for templates and installed apps are configured.
4. **Create an HTML Page**: Develop a template with static HTML and placeholders for dynamic data.
5. **Pass Variables**: Pass the template name and a context dictionary to the render function.

Let's walk through these steps with an example of creating an about page for a fictional website, Little Lemon.

#### Step 1: Define a View Function

In your Django app, open `views.py` and define the `about` view function:

```python
from django.shortcuts import render

def about(request):
    about_content = {
        "about": "Little Lemon is a charming bistro serving fresh and delicious meals."
    }
    return render(request, 'about.html', about_content)
```

#### Step 2: Update URL Configurations

Ensure your view is accessible via a URL. Update `urls.py`:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('about/', views.about, name='about'),
]
```

#### Step 3: Configure Settings

Verify your settings for templates and installed apps in `settings.py`. Ensure the `DIRS` key points to the template directory:

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

#### Step 4: Create an HTML Page

Create an `about.html` file in the `templates` directory:

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

#### Step 5: Pass Variables to the Template

In the view function `about`, we already passed the `about_content` dictionary to the template. The dictionary's key-value pairs are accessible in the template using the `{{ key }}` syntax.

### Testing the Template

1. **Run the Server**: Start your Django server.
   ```bash
   python manage.py runserver
   ```

2. **Access the Page**: Open your web browser and navigate to `http://localhost:8000/about/`.

You should see the "About" heading and the dynamic content from the `about_content` dictionary displayed in a paragraph.

### Adding Dynamic Content

To modify the dynamic content:

1. **Update the Dictionary**: Change the value in the `about_content` dictionary in `views.py`.
   ```python
   about_content = {
       "about": "Little Lemon is a cozy bistro offering a variety of fresh and delicious meals. Visit us for an unforgettable dining experience."
   }
   ```

2. **Save and Refresh**: Save the changes, refresh the web page, and see the updated content.

### Summary

In this guide, you learned how to create and render templates in Django. By defining view functions, configuring URLs, and using Django's template language, you can efficiently manage dynamic content and static HTML. Templates help separate the presentation layer from the business logic, promoting better code organization and reusability.

# Exercise: Creating Templates

Goal
- The learner will practice using Templates in Django

Objectives
- The learner will create views for Menu and About page

- The learner will create respective templates for the two views

- The learner will pass the dictionary and other static content such as images inside the webpages created
