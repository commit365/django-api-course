# Lesson 1: Introduction to Django

## Overview of Django and Its Architecture

Django is a high-level Python web framework that encourages rapid development and clean, pragmatic design. It was created to help developers build web applications quickly and efficiently, following the "Don't Repeat Yourself" (DRY) principle and the "Convention over Configuration" philosophy.

### Key Features of Django

- **MTV Architecture**: Django follows the Model-Template-View (MTV) architectural pattern, which separates data (Model), presentation (Template), and business logic (View). This separation allows for better organization and maintainability of code.
  
- **Built-in Admin Interface**: Django comes with a powerful admin interface that allows developers to manage application data easily without writing additional code.

- **ORM (Object-Relational Mapping)**: Django's ORM allows you to interact with your database using Python objects instead of SQL queries, making database operations more intuitive.

- **Security**: Django includes built-in protection against common security threats, such as SQL injection, cross-site scripting (XSS), and cross-site request forgery (CSRF).

- **Scalability**: Django is designed to handle high traffic loads and can scale easily with your application’s growth.

### Django Components

1. **Model**: Represents the data structure. Models define the fields and behaviors of the data you’re storing. Django uses the ORM to interact with the database.
  
2. **Template**: Manages the presentation layer. Templates are HTML files that can include dynamic content using Django's template language.

3. **View**: Contains the business logic. Views process user requests, interact with the model, and return the appropriate response, often rendering a template.

4. **URL Dispatcher**: Maps URLs to views. Django uses a URL dispatcher to direct incoming web requests to the appropriate view based on the URL.

## Setting Up the Development Environment

To start developing with Django, you need to set up your development environment. Follow these steps to get started:

### Step 1: Install Python

Ensure you have Python installed on your machine. Django requires Python 3.6 or higher. You can download Python from the official website: [python.org](https://www.python.org/downloads/).

To check if Python is installed, run the following command in your terminal:

```bash
python --version
```

If Python is installed, you should see the version number.

### Step 2: Install pip

`pip` is the package installer for Python. It usually comes pre-installed with Python. To check if `pip` is installed, run:

```bash
pip --version
```

If you don't have `pip`, you can install it by following the instructions on the [pip installation page](https://pip.pypa.io/en/stable/installation/).

### Step 3: Create a Virtual Environment

It's a good practice to create a virtual environment for your Django projects to manage dependencies separately. Use the following commands:

1. Navigate to your desired project directory:

   ```bash
   cd /path/to/your/project/directory
   ```

2. Create a virtual environment:

   ```bash
   python -m venv myenv
   ```

   Replace `myenv` with your preferred environment name.

3. Activate the virtual environment:

   - On Windows:

     ```bash
     myenv\Scripts\activate
     ```

   - On macOS/Linux:

     ```bash
     source myenv/bin/activate
     ```

### Step 4: Install Django

With the virtual environment activated, you can install Django using `pip`:

```bash
pip install django
```

To verify that Django has been installed successfully, run:

```bash
python -m django --version
```

You should see the version number of Django.

### Step 5: Create Your First Django Project

Now that your environment is set up, you can create your first Django project. Run the following command:

```bash
django-admin startproject myproject
```

Replace `myproject` with your desired project name. This command creates a new directory with the project structure and necessary files.

### Step 6: Run the Development Server

Navigate into your project directory:

```bash
cd myproject
```

Run the development server:

```bash
python manage.py runserver
```

You should see output indicating that the server is running. Open your web browser and go to `http://127.0.0.1:8000/`. You should see a welcome page from Django, confirming that everything is set up correctly.

## Conclusion

In this lesson, you learned about the basics of Django, its architecture, and how to set up your development environment. You are now ready to start building your first Django application! In the next lesson, we will dive deeper into creating your first Django project and understanding its structure.