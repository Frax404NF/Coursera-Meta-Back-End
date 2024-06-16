# Regular expressions in URLs

### Using Regular Expressions to Define URL Patterns in Django

Django allows developers to create dynamic and complex URL patterns using regular expressions (regex). This guide will demonstrate how to use regex to create and validate dynamic URLs in Django, providing flexibility in URL matching.

#### Understanding Regular Expressions

Regular expressions (regex) are a powerful tool used to define search patterns. They can be used to validate, search, and extract data from strings. Here are some common regex symbols:

- `^`: Anchors the regex at the start of the string.
- `$`: Anchors the regex at the end of the string.
- `[ ]`: Defines a character set.
- `{ }`: Specifies the exact number of occurrences.
- `( )`: Groups parts of the regex.

#### Example: Creating a Dynamic Menu Item Page

Suppose you need to create a menu item page for the Little Lemon website that displays content based on the menu item ID provided in the URL. Instead of creating a static page for each item, you can create a single dynamic page.

1. **Define URL Patterns with Regex:**

   Open your `urls.py` file and define a URL pattern using the `re_path` function from `django.urls`.

   ```python
   from django.urls import path, re_path
   from . import views

   urlpatterns = [
       path('menu_item/10/', views.static_menu_item, name='static_menu_item'),
       re_path(r'^menu_item/(?P<id>\d{1,2})/$', views.dynamic_menu_item, name='dynamic_menu_item'),
   ]
   ```

   In this example:
   - `path('menu_item/10/', ...)` is a static URL pattern.
   - `re_path(r'^menu_item/(?P<id>\d{1,2})/$', ...)` is a dynamic URL pattern using regex to capture a 1 or 2-digit ID.

2. **Create the View Function:**

   Open your `views.py` file and create a view function that matches the regex URL pattern. This function will receive the captured ID as an argument.

   ```python
   from django.http import HttpResponse

   def dynamic_menu_item(request, id):
       items = {
           '1': 'Delicious Italian pasta.',
           '2': 'Crispy Middle Eastern falafel.',
           '10': 'Creamy New York cheesecake.',
       }
       description = items.get(id, 'Item not found.')
       return HttpResponse(f"<h1>Menu Item {id}</h1><p>{description}</p>")

   def static_menu_item(request):
       return HttpResponse("<h1>Static Menu Item</h1><p>This is a static menu item.</p>")
   ```

3. **Running the Server:**

   Ensure your server is running. Open your browser and navigate to URLs like `http://localhost:8000/menu_item/1/`, `http://localhost:8000/menu_item/2/`, or `http://localhost:8000/menu_item/10/` to see the corresponding descriptions.

### Understanding the Regex URL Pattern

The `re_path` function allows for complex URL patterns using regex:

- `r'^menu_item/(?P<id>\d{1,2})/$'`:
  - `^`: Start of the string.
  - `menu_item/`: Matches the literal string "menu_item/".
  - `(?P<id>\d{1,2})`: Captures 1 or 2 digits as a named group `id`.
  - `$`: End of the string.

### Summary

Using regex for URL patterns in Django allows you to create flexible and dynamic URLs. This is particularly useful when you need to match more complex patterns or validate URL parameters before passing them to view functions.

By understanding and practicing regex, you can create robust URL patterns that ensure your Django applications handle routing and parameter validation efficiently.

# URL Namespacing and Views

### URL Namespacing and Views in Django

In Django, URL namespacing is a technique that helps in resolving the same URL name in more than one app by creating a unique namespace for each app. This is particularly useful when you have multiple apps within a project, each having views with potentially conflicting names. Here’s a detailed explanation on how to implement URL namespacing and use it effectively in your Django projects.

#### Defining URL Patterns in Django

Each Django app has a `urls.py` file that defines its URL patterns. These patterns are defined using the `path()` function, which maps a URL path to a view function. Here’s an example:

```python
# demoapp/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('login/', views.login, name='login')
]
```

These URL patterns are included in the project's main `urls.py` file using the `include()` function:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('demo/', include('demoapp.urls')),
]
```

#### Using `reverse()` Function

Hard-coding URLs in your templates and views can make your application less scalable and difficult to maintain. Instead, you can use the `reverse()` function to dynamically get the URL path from the name parameter used in the `path()` function.

Start the Django shell to see this in action:

```bash
python manage.py shell
```

In the shell, you can use the `reverse()` function:

```python
from django.urls import reverse
print(reverse('index'))
# Output: '/demo/'
```

#### Handling Conflicting View Names with Namespaces

When you have view functions with the same name in multiple apps, you can use namespaces to differentiate them. This is done by defining an `app_name` variable in each app’s `urls.py` file:

```python
# demoapp/urls.py
from django.urls import path
from . import views

app_name = 'demoapp'
urlpatterns = [
    path('', views.index, name='index'),
    path('login/', views.login, name='login')
]
```

Now, you can obtain the URL path of the `index` view by prepending the namespace to it:

```python
print(reverse('demoapp:index'))
# Output: '/demo/'
```

If you add another app, for example `newapp`, with a similar structure:

```python
# newapp/urls.py
from django.urls import path
from . import views

app_name = 'newapp'
urlpatterns = [
    path('', views.index, name='index'),
]
```

And update the project's `urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('demo/', include('demoapp.urls')),
    path('newdemo/', include('newapp.urls')),
]
```

You can differentiate the index views using namespaces:

```python
print(reverse('demoapp:index'))
# Output: '/demo/'

print(reverse('newapp:index'))
# Output: '/newdemo/'
```

#### Instance Namespaces

You can also use instance namespaces by providing a namespace parameter in the `include()` function:

```python
# in demoproject/urls.py
urlpatterns = [
    path('demo/', include('demoapp.urls', namespace='demoapp')),
]
```

#### Using Namespaces in Views

To redirect a user to the login view from within another view, use the `reverse()` function along with `HttpResponsePermanentRedirect`:

```python
from django.http import HttpResponsePermanentRedirect
from django.urls import reverse

def myview(request):
    # ...
    return HttpResponsePermanentRedirect(reverse('demoapp:login'))
```

#### Using Namespaces in Templates

In templates, use the `url` tag to dynamically obtain the URL path. This avoids hard-coding URLs:

```html
<form action="{% url 'demoapp:login' %}" method="post">
    <!-- form attributes -->
    <input type="submit">
</form>
```

This ensures that the URL path is dynamically generated based on the view name and namespace, making your application more maintainable and scalable.

### Summary

URL namespacing in Django is a powerful feature that helps manage and resolve conflicts between apps with similar view names. By using application and instance namespaces, you can ensure that your URLs are unique and easily maintainable, which is crucial for larger projects. The `reverse()` function and the `url` template tag are essential tools for dynamically generating URL paths, enhancing the scalability and flexibility of your Django applications.

# Exercise: Creating URLs and Mapping to Views

Goal
- To create more than one view, add their respective URL configurations and display contents on a webpage.

Objectives
- Add URL configurations at the app level for multiple views
```python
from django.urls import path, include
from . import views

urlpatterns = [
        path('', views.home, name="home"),
        path('menu/', views.menu, name="menu"),
        path('about/', views.about, name="about"),
        path('book/', views.book, name="book"),
] 
```
- Learn how to create multiple views inside the views.py
```py
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def home(request):
    return HttpResponse("Welcome to Little Lemon !")

def about(request):
    return HttpResponse("About us")

def menu(request):
    return HttpResponse("Menu for Little Lemon")

def book(request):
    return HttpResponse("Make a booking")

```

# Error Handling

### Understanding Error Handling in Django

#### Overview
Error handling is a crucial aspect of web application development. Despite extensive testing and QA efforts, applications may still encounter errors due to various factors, including network unreliability. This guide will explore error handling in Django, focusing on common client and server error responses and how to manage them using Django’s error handling views.

#### HTTP Response Status Codes
HTTP responses use status codes to indicate the outcome of a request. These codes are categorized as follows:
- **1xx:** Informational responses
- **2xx:** Success responses
- **3xx:** Redirection messages
- **4xx:** Client errors
- **5xx:** Server errors

#### Common Client Error Responses
Client error responses (4xx status codes) indicate issues with the client’s request:
1. **400 Bad Request:** The request parameters are incorrect or malformed.
2. **401 Unauthorized:** Authentication is required to access the requested resource.
3. **403 Forbidden:** The server understands the request, but the client lacks the necessary permissions.
4. **404 Not Found:** The requested resource is not available at the specified location.

#### Common Server Error Responses
Server error responses (5xx status codes) indicate issues on the server:
1. **500 Internal Server Error:** A generic error indicating the server failed to process the request due to an unexpected condition.
   
#### Handling Errors in Django
Django handles errors by raising exceptions, which can be managed via error handling views. These views are specified in a separate `views.py` file at the project level. Django provides four key variables for handling different types of errors:
- **handler400:** Manages 400 Bad Request errors.
- **handler403:** Manages 403 Forbidden errors.
- **handler404:** Manages 404 Not Found errors.
- **handler500:** Manages 500 Internal Server errors.

#### Configuring Error Handling Views
To customize error handling views, follow these steps:
1. **Create Custom Views:** Define custom views in your `views.py` file. These views should accept the appropriate request argument and return the relevant HTTP response subclass.
    - Example:
      ```python
      from django.http import HttpResponseBadRequest, HttpResponseNotFound, HttpResponseForbidden, HttpResponseServerError

      def custom_bad_request_view(request, exception):
          return HttpResponseBadRequest('Bad Request: Custom Message')

      def custom_permission_denied_view(request, exception):
          return HttpResponseForbidden('Permission Denied: Custom Message')

      def custom_page_not_found_view(request, exception):
          return HttpResponseNotFound('Page Not Found: Custom Message')

      def custom_server_error_view(request):
          return HttpResponseServerError('Server Error: Custom Message')
      ```

2. **Configure URL Configuration:** Set the error handling variables in the root URL configuration file (`urls.py`) to point to your custom views.
    - Example:
      ```python
      from django.conf.urls import handler400, handler403, handler404, handler500
      from . import views

      handler400 = 'myapp.views.custom_bad_request_view'
      handler403 = 'myapp.views.custom_permission_denied_view'
      handler404 = 'myapp.views.custom_page_not_found_view'
      handler500 = 'myapp.views.custom_server_error_view'
      ```

3. **Return Appropriate HTTP Response Subclasses:** Use the appropriate HTTP response subclasses in your custom views.
    - **HttpResponseBadRequest:** Returns a 400 status code.
    - **HttpResponseNotFound:** Returns a 404 status code.
    - **HttpResponseForbidden:** Returns a 403 status code.
    - **HttpResponseServerError:** Returns a 500 status code.

#### Summary
By understanding and implementing error handling in Django, developers can create more robust applications that gracefully manage client and server errors. This involves using Django's built-in mechanisms for handling exceptions and customizing error views to enhance user experience and align with the site's design.

In this video, you learned about the importance of error handling, the common HTTP status codes, and how to configure and customize error handling views in Django to improve your web application's reliability and user experience.

# Handling errors in views
### Handling Errors in Django Views

In Django applications, views handle the core logic of processing requests and returning responses. Understanding how to handle errors in views is essential for creating robust and user-friendly web applications. This guide covers the basics of error handling in Django views, including common HTTP response codes, using exceptions, and customizing error pages.

#### HTTP Response and Status Codes
Django views return `HttpResponse` objects, which include a status code indicating the outcome of the request. Common status codes include:
- **200 OK:** Successful request
- **400 Bad Request:** Client-side error in the request
- **403 Forbidden:** Request understood but denied by the server
- **404 Not Found:** Requested resource not found
- **500 Internal Server Error:** General server error

#### Returning Specific Status Codes
To return specific error responses, Django provides predefined classes and allows setting status codes directly in `HttpResponse`.

**Example: Using Predefined Classes**
```python
from django.http import HttpResponse, HttpResponseNotFound

def my_view(request):
    if condition:
        return HttpResponseNotFound('<h1>Page not found</h1>')
    else:
        return HttpResponse('<h1>Page was found</h1>')
```

**Example: Setting Status Code in HttpResponse**
```python
from django.http import HttpResponse

def my_view(request):
    if condition:
        return HttpResponse('<h1>Page not found</h1>', status=404)
    else:
        return HttpResponse('<h1>Page was found</h1>')
```

#### Raising Http404 Exception
A common cause of a 404 error is an incorrect URL entered by the user. Instead of returning `HttpResponseNotFound`, you can raise the `Http404` exception for a more standardized approach.

**Example: Raising Http404**
```python
from django.http import Http404, HttpResponse
from .models import Product

def detail(request, id):
    try:
        product = Product.objects.get(pk=id)
    except Product.DoesNotExist:
        raise Http404("Product does not exist")
    return HttpResponse("Product Found")
```

#### Custom Error Pages
To customize the error page for a 404 error, create a `404.html` file and place it in the `project/templates` directory. Django will use this template when rendering a 404 error response.

**Example: Custom 404 Template**
1. Create `404.html` in `project/templates`.
2. Set `DEBUG = False` in `settings.py` to enable custom error pages.

```python
# settings.py
DEBUG = False
```

#### Exception Classes
Django defines various exception classes in the `django.core.exceptions` module to handle different error scenarios.

**Common Exceptions**
- **ObjectDoesNotExist:** Base exception for `DoesNotExist` exceptions.
- **EmptyResultSet:** Raised when a query returns no results.
- **FieldDoesNotExist:** Raised when a model field does not exist.
- **MultipleObjectsReturned:** Raised when a query returns multiple objects when one was expected.
- **PermissionDenied:** Raised when a user lacks permission for an action.
- **ViewDoesNotExist:** Raised when a requested view does not exist.

**Example: Handling FieldDoesNotExist**
```python
from django.core.exceptions import FieldDoesNotExist
from django.http import HttpResponse

def my_view(request):
    try:
        field = model._meta.get_field(field_name)
    except FieldDoesNotExist:
        return HttpResponse("Field does not exist")
```

**Example: Handling PermissionDenied**
```python
from django.core.exceptions import PermissionDenied
from django.http import HttpResponse

def my_view(request):
    if not request.user.has_perm('myapp.view_mymodel'):
        raise PermissionDenied()
    return HttpResponse()
```

#### Handling Form Validation Errors
Django's Form API includes built-in validators for various field types. Use the `is_valid()` method to check form data and handle invalid data appropriately.

**Example: Handling Form Validation Errors**
```python
from django.http import HttpResponse
from .forms import MyForm

def my_view(request):
    if request.method == "POST":
        form = MyForm(request.POST)
        if form.is_valid():
            # Process the form data
            pass
        else:
            return HttpResponse("Form submitted with invalid data")
```

#### Conclusion
Error handling in Django views involves returning appropriate HTTP status codes, raising exceptions, and customizing error pages to enhance user experience. By understanding and implementing these practices, you can create more reliable and user-friendly Django applications.

# Demo: Handle errors in views

### Handling Errors in Django Views

When navigating the web, users often encounter "Page Not Found" errors (404 errors). In Django, handling these errors gracefully and customizing error pages can enhance user experience. This guide explains how to handle errors in views using Django and create a custom 404 error page.

#### Default 404 Error Page

By default, Django displays a standard 404 error page when a URL cannot be matched. This page includes technical details intended for developers, which are not useful for end-users. Customizing this page can provide a more user-friendly experience.

#### Running the Django Server
To start the Django server, use the following command:
```bash
python3 manage.py runserver
```
Open `localhost` in your browser to see the default Django page. Navigating to an invalid URL, like `/home`, will display the default "Page Not Found" error page with debugging information if `DEBUG = True` in the Django settings file.

#### Switching Debug Mode
Debug mode shows detailed error pages during development. For production, set `DEBUG = False` to prevent end-users from seeing technical details.
1. **Open `settings.py`** and set:
   ```python
   DEBUG = False
   ```
2. **Configure `ALLOWED_HOSTS`**:
   ```python
   ALLOWED_HOSTS = ['*']  # Use specific domain names in production
   ```
3. **Save the file** and **refresh the browser**. A standard 404 page stating "The requested resource was not found on the server" will be displayed.

#### Creating a Custom 404 Error Page
To create a custom 404 error page:

1. **Create `views.py` at the project level** if it doesn't exist.

2. **Update `urls.py`** with necessary imports and URL patterns:
   ```python
   from django.conf.urls import handler404
   from myproject.views import custom_404_view

   handler404 = 'myproject.views.custom_404_view'
   ```

3. **Define the custom view** in `views.py`:
   ```python
   from django.http import HttpResponseNotFound

   def custom_404_view(request, exception):
       return HttpResponseNotFound('<h1>404 - Page not found</h1>')
   ```

4. **Save the files** and **refresh the browser**. Navigating to a non-existent URL will now show your custom 404 error message.

#### Verifying the Custom 404 Page
To verify the custom 404 page and its status code:
1. **Open the browser's developer tools** (right-click and select "Inspect").
2. **Go to the Network tab**.
3. **Navigate to a non-existent URL** and observe the 404 status code in the network request.

#### Adding HTML Elements to Custom 404 Page
Enhance the custom error page with HTML elements for better user experience:
```python
from django.http import HttpResponseNotFound

def custom_404_view(request, exception):
    return HttpResponseNotFound('''
        <h1>404 - Page not found</h1>
        <p>Sorry, the page you are looking for does not exist.</p>
        <a href="/">Go back to Homepage</a>
    ''')
```

#### Best Practices for Custom Error Pages
- **Make them user-friendly**: Use clear language and helpful links.
- **Consistent branding**: Match the site's design and style.
- **Provide navigation**: Include links to the homepage or other helpful sections.

### Summary
In this guide, you learned how to handle errors in Django views and create a custom 404 error page using the `HttpResponseNotFound` class. Custom error pages improve user experience by providing clear, branded messages and helpful navigation options when users encounter errors.

### Practical Steps
1. **Set `DEBUG = False` and configure `ALLOWED_HOSTS`** in `settings.py`.
2. **Create and configure custom 404 views** in `views.py` and `urls.py`.
3. **Verify the custom 404 page** using browser developer tools.
4. **Enhance the custom error page** with HTML for better user experience.

# Class-based views

### Understanding Class-Based Views in Django

Class-Based Views (CBVs) in Django offer a powerful alternative to Function-Based Views (FBVs) by leveraging object-oriented programming (OOP) principles. This guide will explain the concept of CBVs, their benefits, and how to use them effectively, including the use of mixins for reusable components.

#### Introduction to Class-Based Views

- **Class-Based Views (CBVs):** Allow you to use views as objects, offering an alternative to function-based views.
- **Function-Based Views (FBVs):** Require conditional logic (e.g., `if-else` statements) to handle different HTTP methods (GET, POST, etc.).

#### Advantages of Class-Based Views

1. **Simplified Code:**
   - CBVs respond to HTTP requests using class instance methods, avoiding the need for complex conditional logic in FBVs.
   - Separate logic for different HTTP methods (`get`, `post`, etc.) into distinct methods, making the code easier to read and maintain.

2. **Object-Oriented Techniques:**
   - **Inheritance:** Allows you to derive new views from existing ones, promoting code reuse and a clear hierarchy.
   - **Mixins:** Provide reusable components to extend the functionality of CBVs.

#### Implementing Class-Based Views

**Example of a Simple Class-Based View:**
```python
from django.http import HttpResponse
from django.views import View

class MyView(View):
    def get(self, request):
        return HttpResponse('<h1>This is a GET request</h1>')

    def post(self, request):
        return HttpResponse('<h1>This is a POST request</h1>')
```

In this example, `MyView` inherits from `View`, and separate methods (`get` and `post`) handle GET and POST requests.

#### Using Mixins with Class-Based Views

Mixins are a way to add functionality to CBVs by combining multiple classes. They are particularly useful for common operations such as creating, listing, retrieving, updating, and deleting model instances.

**Common Mixins in Django:**
- **CreateMixin:** For creating model instances.
- **ListMixin:** For listing a queryset.
- **RetrieveMixin:** For retrieving a model instance.
- **UpdateMixin:** For updating a model instance.
- **DeleteMixin:** For deleting a model instance.

**Example: Using Mixins**
```python
from django.views.generic import View
from django.http import HttpResponse
from myapp.models import MyModel
from django.views.generic.edit import CreateView, UpdateView
from django.views.generic.list import ListView
from django.views.generic.detail import DetailView
from django.views.generic.edit import DeleteView

class MyCreateView(CreateView):
    model = MyModel
    template_name = 'myapp/my_template.html'
    fields = ['field1', 'field2']

class MyListView(ListView):
    model = MyModel
    template_name = 'myapp/my_template.html'

class MyDetailView(DetailView):
    model = MyModel
    template_name = 'myapp/my_template.html'

class MyUpdateView(UpdateView):
    model = MyModel
    template_name = 'myapp/my_template.html'
    fields = ['field1', 'field2']

class MyDeleteView(DeleteView):
    model = MyModel
    template_name = 'myapp/my_template.html'
    success_url = '/success/'
```

In this example, different views (`MyCreateView`, `MyListView`, `MyDetailView`, `MyUpdateView`, `MyDeleteView`) handle specific operations, leveraging mixins to simplify and organize the code.

#### Best Practices for Using Mixins

- **Use Wisely:** While mixins can simplify code, using too many can make the code harder to read and maintain.
- **Combine Thoughtfully:** Not all mixins should be used together; consider the specific needs of your view when selecting mixins.

#### Conclusion

Class-Based Views in Django provide a structured and scalable way to handle complex view logic. By using object-oriented techniques such as inheritance and mixins, developers can create reusable, maintainable, and organized code. Implementing CBVs can greatly enhance the clarity and efficiency of your Django projects.

# Module summary: Views

### Module Recap: Views in Django

#### Overview
In this module, you've gained a comprehensive understanding of Django views, including creating views, handling HTTP requests and responses, mapping URLs, and dealing with errors. Let's summarize the key points and concepts covered.

#### Key Concepts and Techniques

1. **Understanding Views**
   - **Definition:** A view handles web requests and returns web responses.
   - **Function-Based Views (FBVs):** Create a view function and map it to a URL using routing.
   - **Class-Based Views (CBVs):** Use object-oriented techniques to create reusable and organized view logic.

2. **Creating and Mapping Views**
   - **Routing:** Map a view to a URL using Django's URL configuration file (`urls.py`).
   - **URL Configuration:** Understand the importance of `urls.py` at both the project and app levels.
   - **VS Code Integration:** Create views and map them using VS Code.

3. **HTTP Methods and Parameters**
   - **HTTP Methods:** Get, Put, Post, and Delete.
   - **Parameters:**
     - **Path Parameters:** Part of the URL path.
     - **Query Parameters:** Key-value pairs in the URL query string.
     - **Body Parameters:** Data sent in the body of a POST or PUT request.

4. **Request and Response Objects**
   - **HTTP Request Object:** Used to map URLs to common CRUD operations and get information from the client.
   - **HTTP Response Object:** Used to return data to the client.

5. **Regular Expressions and URL Patterns**
   - **Regular Expressions:** Create different URL patterns.
   - **URL Dispatcher:** Map URLs to views using patterns.

6. **Error Handling**
   - **HTTP Status Codes:** Learn about common status codes (e.g., 100, 200, 400, 404, 500).
   - **Error Responses:** Handle error responses from the server.
   - **Custom Error Pages:** Create custom 404 error pages and other status-specific pages.

7. **Class-Based Views (CBVs)**
   - **Advantages of CBVs:** Simplify code by removing conditional logic and using class instance methods.
   - **Object-Oriented Techniques:** Use inheritance and mixins to create reusable view logic.
   - **Instance Methods:** Implement GET, POST, PUT, and DELETE operations using instance methods.

#### Practical Applications

- **Dynamic Websites:** Use views to handle data processing and template rendering for dynamic content.
- **CRUD Operations:** Implement common operations like creating, retrieving, updating, and deleting database records.
- **Error Handling:** Provide user-friendly error pages and handle exceptions gracefully in your views.
- **URL Mapping:** Create clean and readable URL structures for your application.

#### Conclusion

Congratulations on completing the module on views in Django! You have learned to:
- Create and map views to URLs.
- Handle HTTP requests and responses.
- Use parameters with different HTTP methods.
- Implement error handling in views.
- Utilize class-based views for cleaner and more maintainable code.

With these skills, you are now well-equipped to develop robust web applications using Django's view system. Keep practicing and applying these concepts to build efficient and scalable web projects. Well done!

# Additional Resources

Here is a list of additional resources that may be helpful as you continue your learning journey.

The django.urls functions for use in URLconfs - official documentation
- https://docs.djangoproject.com/en/4.1/ref/urls/

URL dispatcher
- https://docs.djangoproject.com/en/4.1/topics/http/urls/#url-dispatcher

URLs - Official
- https://docs.djangoproject.com/en/4.1/topics/http/urls/#how-django-processes-a-request

Regular Expressions
- https://docs.djangoproject.com/en/4.1/topics/http/urls/#using-regular-expressions

Django URL mapping
- https://docs.djangoproject.com/en/4.1/topics/http/views/#mapping-urls-to-views

Get URL parameters in Django
- https://docs.djangoproject.com/en/4.1/topics/http/urls/#how-django-processes-a-request

Render HTML Forms – GET and POST in Django
- https://docs.djangoproject.com/en/4.1/topics/forms/

HTTP Request Response object
- https://docs.djangoproject.com/en/4.1/ref/request-response/

  
