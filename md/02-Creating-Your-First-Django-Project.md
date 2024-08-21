# Lesson 2: Creating Your First Django Project

In this lesson, we will walk through the process of creating your first Django project. You'll learn how to use the Django command-line interface to set up your project and understand the structure and settings of a Django project.

## Using the Django Command-Line Interface

Django provides a powerful command-line interface (CLI) that allows you to interact with your project easily. Here’s how to create a new Django project using the CLI.

### Step 1: Open Your Terminal

Make sure your virtual environment is activated. If you haven’t activated it yet, navigate to your project directory and run:

- On Windows:

  ```bash
  myenv\Scripts\activate
  ```

- On macOS/Linux:

  ```bash
  source myenv/bin/activate
  ```

### Step 2: Create a New Django Project

To create a new Django project, use the `django-admin startproject` command followed by your project name. For example, to create a project named `myfirstproject`, run:

```bash
django-admin startproject myfirstproject
```

This command will create a new directory named `myfirstproject` containing the initial project files.

### Step 3: Navigate to Your Project Directory

Change into your newly created project directory:

```bash
cd myfirstproject
```

### Step 4: Run the Development Server

To verify that your project was created successfully, you can run the development server. Use the following command:

```bash
python manage.py runserver
```

You should see output indicating that the server is running. Open your web browser and go to `http://127.0.0.1:8000/`. You should see the Django welcome page, confirming that your project is set up correctly.

## Understanding Project Structure and Settings

Now that you have created your first Django project, let’s explore the structure of the project and understand its components.

### Project Structure

When you create a new Django project, the following files and directories are generated:

```
myfirstproject/
    manage.py
    myfirstproject/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

#### 1. `manage.py`

- This is a command-line utility that lets you interact with your Django project. You can use it to run the development server, create applications, apply database migrations, and more.

#### 2. `myfirstproject/` (Inner Directory)

This directory has the same name as your project and contains the core settings and configuration files.

- **`__init__.py`**: This file indicates that this directory should be treated as a Python package. It can be empty.

- **`settings.py`**: This file contains all the settings and configurations for your Django project, such as database configuration, installed apps, middleware, and more. You can customize your project settings here.

- **`urls.py`**: This file is used to define the URL patterns for your project. You will map URLs to views in this file, allowing you to control what content is displayed for different URLs.

- **`asgi.py`**: This file is for configuring ASGI (Asynchronous Server Gateway Interface) applications. It is used for handling asynchronous requests.

- **`wsgi.py`**: This file is for configuring WSGI (Web Server Gateway Interface) applications. It is used for deploying your application to production servers.

### Step 5: Exploring `settings.py`

Open the `settings.py` file in your preferred text editor to familiarize yourself with the configuration options. Here are a few key sections:

- **`BASE_DIR`**: This variable defines the base directory of your project, which is used to construct paths to other files.

- **`DEBUG`**: This setting determines whether the project is in debug mode. It should be set to `True` during development and `False` in production.

- **`INSTALLED_APPS`**: This list contains all the applications that are enabled in your project. You can add third-party apps or your own applications here.

- **`DATABASES`**: This section defines the database configuration. By default, Django uses SQLite, but you can change it to other databases like PostgreSQL or MySQL.

- **`MIDDLEWARE`**: This list contains middleware components that process requests and responses. Middleware can handle tasks such as session management and user authentication.

### Step 6: Exploring `urls.py`

Open the `urls.py` file to see how URL routing is set up. By default, it contains the following code:

```python
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

- The `urlpatterns` list defines the URL patterns for your project. The default configuration includes a route to the Django admin interface.

## Conclusion

In this lesson, you learned how to create your first Django project using the command-line interface. You explored the project structure and key files, including `manage.py`, `settings.py`, and `urls.py`. With this foundational knowledge, you are ready to start building your Django applications! In the next lesson, we will dive into Django models and databases.