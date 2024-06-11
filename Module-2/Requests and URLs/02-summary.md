# HTTP requests
### Understanding HTTP and HTTPS

In this guide, you'll learn about the basics of HTTP and HTTPS, the core protocols used for web communication. You'll explore the structure of HTTP requests and responses, the methods used, and the significance of HTTP status codes. Additionally, you'll understand how HTTPS enhances the security of web communications.

#### What is HTTP?

HTTP (Hypertext Transfer Protocol) is a protocol used for transferring web resources such as HTML documents, images, stylesheets, and other files over the internet. It is the foundational protocol for data exchange on the web.

- **Request-Response Protocol**: HTTP is a request-response protocol where a client (usually a web browser) sends an HTTP request to a server, and the server responds with an HTTP response.

#### Structure of an HTTP Request

An HTTP request consists of:
1. **Method**: The type of action the client wants to perform (e.g., GET, POST, PUT, DELETE).
2. **Path**: The location of the resource on the web server (e.g., `/images/logo.png`).
3. **Version**: The version of the HTTP protocol (e.g., HTTP/1.1).
4. **Headers**: Additional information about the request and the client (e.g., `Content-Type`, `User-Agent`).
5. **Body** (optional): The data sent to the server (used mainly with POST and PUT requests).

#### Common HTTP Methods

- **GET**: Retrieve information from the server.
- **POST**: Send data to the server to create a new resource.
- **PUT**: Update an existing resource on the server.
- **DELETE**: Remove a resource from the server.

#### Structure of an HTTP Response

An HTTP response consists of:
1. **Status Line**: Includes the HTTP version, status code, and status message (e.g., `HTTP/1.1 200 OK`).
2. **Headers**: Additional information about the response (e.g., `Content-Type`, `Content-Length`).
3. **Body**: The actual content of the response, such as an HTML document or image.

#### HTTP Status Codes

HTTP status codes are three-digit numbers that indicate the result of the HTTP request. They are grouped into five categories:

1. **Informational (100-199)**: Provisional responses indicating that the request is being processed (e.g., `100 Continue`).
2. **Successful (200-299)**: Indicate that the request was successfully processed (e.g., `200 OK`).
3. **Redirection (300-399)**: Indicate that the resource has been moved to a different location (e.g., `301 Moved Permanently`).
4. **Client Error (400-499)**: Indicate that there was an error with the request (e.g., `400 Bad Request`, `404 Not Found`).
5. **Server Error (500-599)**: Indicate that there was an error on the server side (e.g., `500 Internal Server Error`).

#### HTTPS: Secure HTTP

- **HTTPS (Hypertext Transfer Protocol Secure)**: The secure version of HTTP, which encrypts the data being transferred between the client and server.
- **Encryption**: HTTPS uses encryption to turn the data into a secret code, which can only be deciphered by the intended recipient. This prevents eavesdroppers from reading the data.
- **Lock Icon**: When you see a lock icon beside the URL in your web browser, it indicates that HTTPS is being used.

#### Key Points

- **HTTP is essential for web communication**: It enables the transfer of web resources between clients and servers.
- **HTTP methods define the type of actions**: Common methods include GET, POST, PUT, and DELETE.
- **HTTP requests and responses have specific structures**: They include methods, paths, versions, headers, and sometimes bodies.
- **HTTP status codes indicate the outcome**: They are grouped into informational, successful, redirection, client error, and server error responses.
- **HTTPS ensures secure communication**: By encrypting data, it protects sensitive information like credit card details during online transactions.

By understanding these fundamentals, you can better grasp how web browsers and servers interact, and how secure communication is achieved on the web.

# Request and Response Objects

### Understanding Django Request and Response Objects

In Django, web applications operate on the principle of a request-response cycle in a client-server architecture, adhering to the HTTP protocol. This involves the browser sending a request, usually in the form of a URL, and the web application responding appropriately based on the data in that request. Django uses `HttpRequest` and `HttpResponse` classes, located in the `django.http` module, to handle this interaction.

#### HttpRequest Object

The `HttpRequest` object in Django encapsulates all the information about the request that the client (e.g., a web browser) has made. This object is created by Django and passed to the view function as its first parameter. Here are some key attributes and methods of the `HttpRequest` object:

1. **request.method**
   - This attribute identifies the HTTP method used by the client. It can be any of the standard HTTP methods such as `GET`, `POST`, `PUT`, `DELETE`.
   - Example:
     ```python
     if request.method == 'GET':
         do_something()
     elif request.method == 'POST':
         do_something_else()
     ```

2. **request.GET and request.POST**
   - These attributes return dictionary-like objects containing parameters sent in the GET and POST requests, respectively.

3. **request.COOKIES**
   - This attribute contains a dictionary of cookies sent by the client.

4. **request.FILES**
   - Contains uploaded files in the form of `UploadedFile` objects when the user uploads files using a multipart form.

5. **request.user**
   - Contains the currently logged-in user as an instance of `django.contrib.auth.models.User`. If the user is not authenticated, it returns `AnonymousUser`.
   - Example:
     ```python
     if request.user.is_authenticated:
         # Do something for logged-in users.
     else:
         # Do something for anonymous users.
     ```

6. **request.has_key(key)**
   - This method checks if the GET or POST dictionary has a value for the given key.

#### HttpResponse Object

Unlike `HttpRequest`, the `HttpResponse` object is created within the view function and is returned to the client. It encapsulates the response data to be sent back to the client.

- **Creating a Basic HttpResponse**
  ```python
  from django.http import HttpResponse
  
  def index(request):
      return HttpResponse("Hello World")
  ```

- **Rendering an HTML Template**
  ```python
  from django.http import HttpResponse
  from django.template import loader
  
  def index(request):
      template = loader.get_template('demoapp/index.html')
      context = {}
      return HttpResponse(template.render(context, request))
  ```

- **Attributes and Methods of HttpResponse**
  - **status_code**: Returns the HTTP status code.
  - **content**: Returns the byte string of the response.
  - **__getitem__()**: Returns the value of a header.
  - **__setitem__()**: Adds a header.
  - **write()**: Creates a file-like object for the response content.

#### Example View Function Demonstrating Request and Response Attributes

Here's a practical example to illustrate the attributes of the `HttpRequest` and `HttpResponse` objects:

```python
from django.http import HttpResponse

def index(request):
    path = request.path
    method = request.method
    content = '''
    <center><h2>Testing Django Request and Response Objects</h2>
    <p>Request path : {}</p>
    <p>Request Method : {}</p></center>
    '''.format(path, method)
    return HttpResponse(content)
```

To see this in action, add the above view function to your `views.py` file in a Django app. Then, configure your URL patterns to point to this view, start the Django development server, and visit `http://localhost:8000/demo/`.

![](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/fhFvvORXTE6K8xzaIjsUlA_07ef40e5abec475998fb0f4252c0eae1_Django-page-loadin-GET-info.png?expiry=1718236800000&hmac=ZGBHTHDmRorWHBOvtgvO1zGr8JroiUpqH4ay5IkQhMo)

This setup will display a simple web page showing the request path and method, demonstrating how you can access request data and create a basic response in Django.

### Summary

- **HttpRequest Object**: Provides detailed information about the client's request, including method, parameters, cookies, files, and user.
- **HttpResponse Object**: Encapsulates the response data sent back to the client, including content, status code, and headers.
- **Request-Response Cycle**: Django's URL dispatcher routes the request to the appropriate view, which processes the request and returns an `HttpResponse`.

Understanding these objects is fundamental to working with Django views and effectively handling web requests and responses.

# Creating Requests and Responses

### Exploring the Django Request-Response Cycle

The request-response cycle in Django follows a systematic flow in a client-server architecture, adhering to the HTTP protocol. Let's break down how this cycle works and how to handle request and response objects in Django.

#### The Request-Response Cycle

1. **Entering a URL**: 
   - The cycle begins when a user enters a URL in their web browser.

2. **URL Processing**:
   - This URL is sent to the web server where Django searches for a matching pattern inside the `urls.py` file.
   - Once it finds the URL, it maps it to its associated view function.

3. **View Function**:
   - Inside the view file, the view function receives the HTTP request as a request object.
   - The view function processes this request and defines an appropriate `HttpResponse`.

4. **Response Generation**:
   - The view sends back an `HttpResponse` object.
   - This response is processed by Django, and the client receives the `HttpResponse`.

#### Practical Implementation in Django

Let's walk through an example of how to work with Django's request and response objects.

1. **Creating the View Function**:

   Open `views.py` and add the necessary imports:

   ```python
   from django.http import HttpResponse

   def home(request):
       # Access the path attribute of the request object
       path = request.path

       # Create a formatted string to include the path
       content = f'<center><h2>Request Path</h2><p>Path: {path}</p></center>'

       # Return an HttpResponse with the content
       return HttpResponse(content, content_type="text/html")
   ```

2. **Updating URL Patterns**:

   Ensure the view function matches the URL pattern in `urls.py`:

   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('home/', views.home, name='home'),
   ]
   ```

3. **Running the Server**:

   Save your files and run the Django development server:

   ```bash
   python manage.py runserver
   ```

   Visit `http://localhost:8000/home/` in your browser to see the result.

#### Modifying HttpResponse Object

You can further manipulate the `HttpResponse` object by adding headers and modifying the response content:

1. **Creating Another HttpResponse**:

   ```python
   def another_response(request):
       response = HttpResponse("This works.")
       return response
   ```

   Add this view to your `urls.py`:

   ```python
   urlpatterns = [
       path('home/', views.home, name='home'),
       path('another/', views.another_response, name='another_response'),
   ]
   ```

   Visiting `http://localhost:8000/another/` will display "This works."

2. **Returning HttpRequest Attributes**:

   You can return various attributes of the `HttpRequest` object:

   ```python
   def request_info(request):
       scheme = request.scheme
       method = request.method
       address = request.META['REMOTE_ADDR']
       user_agent = request.META['HTTP_USER_AGENT']
       path_info = request.path_info

       message = f'''
       <center><h2>Request Info</h2>
       <p>Scheme: {scheme}</p>
       <p>Method: {method}</p>
       <p>Address: {address}</p>
       <p>User Agent: {user_agent}</p>
       <p>Path Info: {path_info}</p></center>
       '''
       return HttpResponse(message, content_type="text/html")
   ```

   Add this view to your `urls.py`:

   ```python
   urlpatterns = [
       path('home/', views.home, name='home'),
       path('another/', views.another_response, name='another_response'),
       path('request-info/', views.request_info, name='request_info'),
   ]
   ```

   Visiting `http://localhost:8000/request-info/` will display the request attributes.

3. **Updating Response Headers**:

   You can also update the response headers:

   ```python
   def update_headers(request):
       response = HttpResponse("Header updated.")
       response.headers['Age'] = '20'
       return response
   ```

   Add this view to your `urls.py`:

   ```python
   urlpatterns = [
       path('home/', views.home, name='home'),
       path('another/', views.another_response, name='another_response'),
       path('request-info/', views.request_info, name='request_info'),
       path('update-headers/', views.update_headers, name='update_headers'),
   ]
   ```

   Visiting `http://localhost:8000/update-headers/` will show "Header updated." and you can check the response headers to see the `Age` header.

### Summary

- **Request-Response Cycle**: Begins with a URL entered by the user, processed by Django to match a view, and ends with a response sent back to the client.
- **HttpRequest Object**: Contains request data like method, path, cookies, files, and user information.
- **HttpResponse Object**: Contains the response data to be sent back, including content, headers, and status code.
- **Django Views**: Use `HttpRequest` and `HttpResponse` to handle and respond to client requests.

This understanding is fundamental for handling web requests and responses effectively in Django applications.

# Understanding URLs

### Understanding the Structure of a URL

When accessing the web, you often click on a web link or type in an address, known as a URL (Uniform Resource Locator). A URL points to a specific resource or web page on the web. This guide will explain what a URL is, its different parts, and how each part is constructed to point to a specific resource.

#### Components of a URL

A URL is composed of multiple parts:
1. **Scheme (Protocol)**
2. **Subdomain**
3. **Domain Name**
4. **File Path**
5. **Parameters (Query Strings)**

Let's explore each part in detail:

#### 1. Scheme (Protocol)

- **Location**: At the beginning of a URL.
- **Common Schemes**: `http://` and `https://`.
- **Function**: Specifies the protocol for data transmission.
  - `HTTP` (Hypertext Transfer Protocol): Sends data as plain text.
  - `HTTPS` (HTTP Secure): Encrypts data for secure transmission.

#### 2. Subdomain

- **Location**: Before the domain name.
- **Common Subdomain**: `www` (World Wide Web).
- **Function**: Typically hosts the home page and other important pages.
- **Note**: Modern browsers may replace the scheme and subdomain with symbols, such as a lock icon for encrypted pages.

#### 3. Domain Name

- **Parts**:
  - **Second-Level Domain (SLD)**: Represents the organization or company name (e.g., `google` in `google.com`).
  - **Top-Level Domain (TLD)**: Indicates the type or location of the organization (e.g., `.com`, `.org`, `.net`, `.ie`).
- **Function**: Identifies the specific organization or entity hosting the resource.

#### 4. File Path

- **Location**: After the domain name.
- **Function**: Directs to the specific location of a resource, such as web documents, image files, or metadata.
- **Types of Resources**: Local (stored on the user's device or server) or Web-based (stored on a remote web server).

#### 5. Parameters (Query Strings)

- **Location**: After the file path, starting with a `?`.
- **Function**: Captures additional information and passes values for processing.
- **Format**: Represented as key-value pairs, separated by `&` if there are multiple parameters.
- **Usage in Django**: Mainly processes URL parameters to handle specific parts of the URL.

### Example URL Breakdown

Consider the URL: `https://www.example.com/path/to/resource?key1=value1&key2=value2`

- **Scheme**: `https`
- **Subdomain**: `www`
- **Second-Level Domain**: `example`
- **Top-Level Domain**: `com`
- **File Path**: `/path/to/resource`
- **Parameters**: `?key1=value1&key2=value2`

### URL Design

URL design is the practice of purposefully creating URLs to define well-structured and meaningful addresses. In Django, understanding URL components is crucial for developing efficient and user-friendly web applications. Proper URL design helps in organizing website content and making it accessible.

### Summary

- A URL is an address that points to a specific resource on the web.
- It consists of a scheme (protocol), subdomain, domain name, file path, and parameters.
- Each part plays a vital role in locating and accessing web resources.
- Understanding URL structure is essential for creating well-designed, user-friendly web applications in Django.

By understanding and utilizing these components, you can effectively navigate, design, and manage web addresses in Django applications.

# Parameters

### Using Parameters in a Django Web Application

When developing a web application, you often need to pass additional data between the client and server. This data can be transmitted using different types of parameters and HTTP methods, such as GET, POST, PUT, and DELETE. In this guide, we'll explore the different options for using parameters in a Django application, including path, query, and body parameters.

#### Path Parameters

Path parameters are included directly in the URL and are part of the endpoint. For example, in the URL `http://example.com/customer/5`, the number `5` is a path parameter.

**Example:**

1. **URL Dispatcher Configuration:**
   
   Add the path to `urls.py`:
   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('getuser/<str:name>/<int:id>', views.pathview, name='pathview'),
   ]
   ```

2. **View Function:**
   
   Define the view in `views.py`:
   ```python
   from django.http import HttpResponse

   def pathview(request, name, id):
       return HttpResponse(f"Name: {name} UserID: {id}")
   ```

3. **Access the URL:**
   
   Visit `http://localhost:8000/getuser/John/1` to see the response.

#### Query Parameters

Query parameters are included in the URL after a `?` symbol and are represented as key-value pairs separated by `&`. For example, in the URL `http://localhost:8000/getuser/?name=John&id=1`, `name=John` and `id=1` are query parameters.

**Example:**

1. **URL Dispatcher Configuration:**
   
   Add the path to `urls.py`:
   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('getuser/', views.qryview, name='qryview'),
   ]
   ```

2. **View Function:**
   
   Define the view in `views.py`:
   ```python
   from django.http import HttpResponse

   def qryview(request):
       name = request.GET['name']
       id = request.GET['id']
       return HttpResponse(f"Name: {name} UserID: {id}")
   ```

3. **Access the URL:**
   
   Visit `http://localhost:8000/getuser/?name=John&id=1` to see the response.

#### Body Parameters

Body parameters are typically sent via HTML forms using the POST method. These parameters are included in the body of the HTTP request rather than in the URL.

**Example:**

1. **Create an HTML Form:**
   
   Save the following HTML form as `form.html` in your `templates` directory:
   ```html
   <form action="/getform/" method="POST">
       {% csrf_token %}
       <p>Name: <input type="text" name="name"></p>
       <p>UserID: <input type="text" name="id"></p>
       <input type="submit">
   </form>
   ```

2. **Show the Form:**
   
   Define a view to render the form in `views.py`:
   ```python
   from django.shortcuts import render

   def showform(request):
       return render(request, 'form.html')
   ```

   Add the path to `urls.py`:
   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('showform/', views.showform, name='showform'),
   ]
   ```

3. **Handle Form Submission:**
   
   Define a view to handle the form data in `views.py`:
   ```python
   from django.http import HttpResponse

   def getform(request):
       if request.method == 'POST':
           name = request.POST['name']
           id = request.POST['id']
           return HttpResponse(f"Name: {name} UserID: {id}")
   ```

   Add the path to `urls.py`:
   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('getform/', views.getform, name='getform'),
   ]
   ```

4. **Access the Form:**
   
   Visit `http://localhost:8000/showform/`, fill out the form, and submit it to see the response.

### Summary

- **Path Parameters**: Included directly in the URL path and typically used to identify resources.
- **Query Parameters**: Included in the URL after a `?` symbol and used to filter or provide additional data.
- **Body Parameters**: Sent in the body of the HTTP request, often used in forms with the POST method for secure data transmission.

Understanding how to use these different types of parameters in Django allows you to handle a wide range of client-server interactions, making your web applications more dynamic and functional.

# Mapping URLs with Params

### Using URL Parameters in Django

Django provides a flexible way to pass values to view functions through URL parameters. This guide will demonstrate how to capture values from URLs and use them in your view functions.

#### Capturing URL Parameters

We will create an example where a web page displays a drink name from the URL.

1. **Define URL Patterns:**

   Open your `urls.py` file and define a URL pattern using the `path` function. Use angle brackets to specify a path converter and the parameter name.

   ```python
   from django.urls import path
   from . import views

   urlpatterns = [
       path('dishes/<str:dish>/', views.menu_item, name='menu_item'),
   ]
   ```

   Here, `str` is the path converter type, and `dish` is the parameter name.

2. **Create the View Function:**

   Open your `views.py` file and create a view function that matches the URL pattern. This function will receive the URL parameter as an argument.

   ```python
   from django.http import HttpResponse

   def menu_item(request, dish):
       items = {
           'pasta': 'Delicious Italian pasta.',
           'falafel': 'Crispy Middle Eastern falafel.',
           'cheesecake': 'Creamy New York cheesecake.',
       }
       description = items.get(dish, 'Dish not found.')
       return HttpResponse(f"<h1>{dish.capitalize()}</h1><p>{description}</p>")
   ```

3. **Running the Server:**

   Ensure your server is running. Open your browser and navigate to URLs like `http://localhost:8000/dishes/pasta/`, `http://localhost:8000/dishes/falafel/`, or `http://localhost:8000/dishes/cheesecake/` to see the corresponding descriptions.

#### Understanding Path Converters

Path converters specify the type of data you expect in the URL parameter. Django provides several built-in converters:

- `str`: Matches any non-empty string (default if no converter is specified).
- `int`: Matches zero or any positive integer.
- `slug`: Matches any slug string (ASCII letters or numbers, hyphens, underscores).
- `uuid`: Matches a formatted UUID.
- `path`: Matches any non-empty string, including path separators (`/`).

#### Using Query Parameters

Query parameters are another way to pass data in the URL. They are included after a `?` symbol and represented as key-value pairs.

**Example:**

1. **Define URL Pattern:**

   Add a new path in `urls.py`:

   ```python
   urlpatterns = [
       path('getuser/', views.qryview, name='qryview'),
   ]
   ```

2. **Create the View Function:**

   ```python
   def qryview(request):
       name = request.GET.get('name', 'Guest')
       id = request.GET.get('id', 'Unknown')
       return HttpResponse(f"Name: {name} UserID: {id}")
   ```

3. **Access the URL:**

   Visit `http://localhost:8000/getuser/?name=John&id=1` to see the response.

#### Using Body Parameters

Body parameters are typically sent via forms using the POST method. They are included in the body of the HTTP request.

**Example:**

1. **Create an HTML Form:**

   Save the following form as `form.html` in your `templates` directory:

   ```html
   <form action="/getform/" method="POST">
       {% csrf_token %}
       <p>Name: <input type="text" name="name"></p>
       <p>UserID: <input type="text" name="id"></p>
       <input type="submit">
   </form>
   ```

2. **Show the Form:**

   Define a view to render the form:

   ```python
   from django.shortcuts import render

   def showform(request):
       return render(request, 'form.html')
   ```

   Add the path in `urls.py`:

   ```python
   urlpatterns = [
       path('showform/', views.showform, name='showform'),
   ]
   ```

3. **Handle Form Submission:**

   ```python
   def getform(request):
       if request.method == 'POST':
           name = request.POST['name']
           id = request.POST['id']
           return HttpResponse(f"Name: {name} UserID: {id}")
   ```

   Add the path in `urls.py`:

   ```python
   urlpatterns = [
       path('getform/', views.getform, name='getform'),
   ]
   ```

4. **Access the Form:**

   Visit `http://localhost:8000/showform/`, fill out the form, and submit it to see the response.

### Summary

- **Path Parameters:** Captured directly in the URL and parsed based on the specified path converter.
- **Query Parameters:** Passed as key-value pairs after a `?` symbol in the URL.
- **Body Parameters:** Sent via forms in the request body, often using the POST method.

By understanding how to use these parameters, you can create dynamic and interactive web applications that handle data efficiently and securely.

# Exercise: Mapping URLs with Params
Objectives
- Learner will define dynamic URL paths using path converters to capture a value from the URL, using angle brackets.

- The learner will practice capturing URL paths and print paths as headings on the webpage.

# Additional resources

1. Creating views - This is the official documentation for Django's views. It provides detailed information on how to create views in Django.

2. Class-based views - This resource is also official documentation that focuses on class-based views in Django. It explains how to use this approach to handle requests and responses.

3. The render() function in Django - This resource covers the render() function in Django, which is used to render templates and return HTTP responses. It explains how to use this function effectively in your Django projects.

4. Getting query parameters from a request in Django - This resource provides information on how to extract query parameters from a request in Django. It explains the process of accessing and utilizing these parameters in your Django application.

5. The HTTP server responses - This resource covers the different types of HTTP server responses in Django. It explains the various response classes and how to use them to send appropriate responses to client requests.

You can access these resources by clicking on the following links:

- [Creating views - official documentation](link-to-resource)
- [Class-based views - Official](link-to-resource)
- [The render() function in Django](link-to-resource)
- [Getting query parameters from a request in Django](link-to-resource)
- [The HTTP server responses](link-to-resource)

Please note that the actual links to these resources are not provided here. You can find them within the course materials or by searching for the specific topics on the official Django documentation website.