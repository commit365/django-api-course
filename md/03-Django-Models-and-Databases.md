# Lesson 3: Django Models and Databases

In this lesson, we will explore Django models and how they interact with databases. You will learn how to define models, create a database schema, and use Django's Object-Relational Mapping (ORM) to interact with your database.

## Defining Models and Database Schema

In Django, a model is a Python class that represents a table in your database. Each attribute of the class corresponds to a column in the table. Django provides a simple way to define models using its built-in fields.

### Step 1: Create a New Django App

Before defining models, it’s a good practice to create a new Django app within your project. An app is a self-contained module that can be reused across projects. To create a new app, run the following command in your terminal:

```bash
python manage.py startapp myapp
```

Replace `myapp` with your desired app name. This command creates a new directory called `myapp` with the necessary files.

### Step 2: Define Your Model

Open the `models.py` file in your new app directory (`myapp/models.py`). Here, you will define your models. For example, let’s create a simple model for a blog post:

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)  # Title of the post
    content = models.TextField()               # Content of the post
    created_at = models.DateTimeField(auto_now_add=True)  # Timestamp when the post was created
    updated_at = models.DateTimeField(auto_now=True)      # Timestamp when the post was last updated

    def __str__(self):
        return self.title  # Return the title when the object is printed
```

### Explanation of the Model Fields

- **`CharField`**: Used for short text fields (e.g., titles). The `max_length` parameter specifies the maximum length of the field.

- **`TextField`**: Used for long text fields (e.g., blog content) where the length is not restricted.

- **`DateTimeField`**: Used for date and time fields. The `auto_now_add` parameter automatically sets the field to the current date and time when the object is created, while `auto_now` updates the field each time the object is saved.

- **`__str__` Method**: This method defines how the model instance is represented as a string. In this case, it returns the title of the post.

### Step 3: Register Your Model

To make your model accessible in the Django admin interface, you need to register it. Open the `admin.py` file in your app directory (`myapp/admin.py`) and add the following code:

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

This code imports the `Post` model and registers it with the Django admin site.

### Step 4: Update `settings.py`

You need to inform Django about your new app. Open the `settings.py` file in your project directory (`myfirstproject/settings.py`) and add your app to the `INSTALLED_APPS` list:

```python
INSTALLED_APPS = [
    ...
    'myapp',  # Add your app here
]
```

### Step 5: Create Database Migrations

Django uses migrations to apply changes to the database schema. To create a migration for your new model, run the following command:

```bash
python manage.py makemigrations
```

This command generates a migration file that describes the changes to be made to the database.

### Step 6: Apply Migrations

To apply the migrations and create the corresponding table in the database, run:

```bash
python manage.py migrate
```

This command updates the database schema based on the migration file.

### Step 7: Access the Django Admin Interface

To see your new model in action, run the development server:

```bash
python manage.py runserver
```

Open your web browser and go to `http://127.0.0.1:8000/admin/`. Log in using the superuser account you created earlier (if you haven’t created one yet, you can do so with the command `python manage.py createsuperuser`). You should see your `Post` model listed under your app. You can now add, edit, and delete blog posts through the admin interface.

## Introduction to Django ORM (Object-Relational Mapping)

Django's ORM allows you to interact with your database using Python objects instead of writing raw SQL queries. This abstraction simplifies database operations and makes your code more readable and maintainable.

### Basic ORM Operations

Here are some basic operations you can perform using Django's ORM:

#### 1. Creating Objects

To create a new instance of a model, you can use the following code in the Django shell:

```bash
python manage.py shell
```

Once in the shell, you can run:

```python
from myapp.models import Post

# Create a new post
new_post = Post(title="My First Post", content="This is the content of my first post.")
new_post.save()  # Save the post to the database
```

#### 2. Querying Objects

You can retrieve objects from the database using various query methods:

```python
# Get all posts
all_posts = Post.objects.all()

# Get a single post by ID
single_post = Post.objects.get(id=1)

# Filter posts by title
filtered_posts = Post.objects.filter(title__icontains="First")
```

#### 3. Updating Objects

To update an existing object, retrieve it, modify its attributes, and save it:

```python
post_to_update = Post.objects.get(id=1)
post_to_update.title = "Updated Title"
post_to_update.save()  # Save the changes
```

#### 4. Deleting Objects

To delete an object, retrieve it and call the `delete()` method:

```python
post_to_delete = Post.objects.get(id=1)
post_to_delete.delete()  # Delete the post from the database
```

### Conclusion

In this lesson, you learned how to define models in Django and create a database schema. You also explored Django's ORM, which allows you to interact with your database using Python objects. With this knowledge, you can now create and manage data in your Django applications effectively. In the next lesson, we will explore the Django admin interface in more detail and learn how to manage our models through it.