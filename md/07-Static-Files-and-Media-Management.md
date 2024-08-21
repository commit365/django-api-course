# Lesson 7: Static Files and Media Management

In this lesson, we will learn how to manage static files and media in Django. Static files include CSS, JavaScript, and images that are part of your web application, while media files refer to user-uploaded content, such as images or documents. We will cover how to serve static files and handle user-uploaded files effectively.

## Serving Static Files (CSS, JavaScript, Images)

### Step 1: Setting Up Static Files

Django provides a way to manage static files through the `STATIC_URL` and `STATICFILES_DIRS` settings. By default, Django looks for static files in a folder named `static` within each app.

1. **Create a Static Directory**: Inside your app directory (`myapp`), create a new directory called `static`:

   ```
   myapp/
       static/
           myapp/
   ```

   The `myapp` subdirectory is used to avoid naming conflicts with static files from other apps.

2. **Add Static Files**: Inside the `myapp/static/myapp/` directory, you can add your CSS, JavaScript, and image files. For example, create a file named `styles.css`:

   ```css
   /* myapp/static/myapp/styles.css */
   body {
       font-family: Arial, sans-serif;
       background-color: #f4f4f4;
       margin: 0;
       padding: 20px;
   }

   h1 {
       color: #333;
   }

   ul {
       list-style-type: none;
       padding: 0;
   }

   li {
       background: #fff;
       margin: 10px 0;
       padding: 10px;
       border: 1px solid #ddd;
   }
   ```

### Step 2: Configuring Static Files in Settings

Open your project’s `settings.py` file and ensure you have the following settings:

```python
# settings.py

# URL to use when referring to static files located in STATIC_ROOT
STATIC_URL = '/static/'

# Additional locations of static files
STATICFILES_DIRS = [
    BASE_DIR / "myapp/static",  # Adjust the path if necessary
]
```

### Step 3: Loading Static Files in Templates

To use static files in your templates, you need to load the static template tag. Update your `post_list.html` to include the CSS file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Blog Posts</title>
    {% load static %}  <!-- Load the static template tag -->
    <link rel="stylesheet" href="{% static 'myapp/styles.css' %}">  <!-- Link to the CSS file -->
</head>
<body>
    <h1>Blog Posts</h1>
    
    <form method="POST">
        {% csrf_token %}
        {{ form.as_p }}
        {% if form.errors %}
            <div class="error">
                <strong>Please correct the errors below:</strong>
                <ul>
                    {% for field in form %}
                        {% for error in field.errors %}
                            <li>{{ field.label }}: {{ error }}</li>
                        {% endfor %}
                    {% endfor %}
                    {% for error in form.non_field_errors %}
                        <li>{{ error }}</li>
                    {% endfor %}
                </ul>
            </div>
        {% endif %}
        <button type="submit">Submit</button>
    </form>

    <ul>
        {% for post in posts %}
            <li>
                <strong>{{ post.title }}</strong><br>
                {{ post.content|truncatewords:20 }}
                <a href="#">Read more</a>
            </li>
        {% empty %}
            <li>No posts available.</li>
        {% endfor %}
    </ul>
</body>
</html>
```

### Step 4: Running the Development Server

Run the development server:

```bash
python manage.py runserver
```

Open your web browser and navigate to `http://127.0.0.1:8000/`. You should see your blog posts styled with the CSS you added.

## Handling User-Uploaded Files (Media)

### Step 1: Configuring Media Settings

To handle user-uploaded files, you need to configure media settings in your `settings.py` file:

```python
# settings.py

# URL to use when referring to media files
MEDIA_URL = '/media/'

# Directory where media files will be stored
MEDIA_ROOT = BASE_DIR / 'media'  # Create a media directory in your project root
```

### Step 2: Creating a File Upload Form

Let’s extend our blog post form to allow users to upload images. Update the `PostForm` in `forms.py` to include an image field:

```python
from django import forms
from .models import Post

class PostForm(forms.ModelForm):
    class Meta:
        model = Post
        fields = ['title', 'content', 'image']  # Add the image field
```

Make sure to update your `Post` model in `models.py` to include an image field:

```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    image = models.ImageField(upload_to='images/', blank=True, null=True)  # Image field

    def __str__(self):
        return self.title
```

### Step 3: Updating the View to Handle Image Uploads

You need to update your view to handle image uploads. Modify the `post_list` view in `views.py`:

```python
def post_list(request):
    posts = Post.objects.all()
    if request.method == 'POST':
        form = PostForm(request.POST, request.FILES)  # Include request.FILES to handle file uploads
        if form.is_valid():
            form.save()
            return redirect('post_list')
    else:
        form = PostForm()

    return render(request, 'myapp/post_list.html', {'posts': posts, 'form': form})
```

### Step 4: Updating the Template to Display Uploaded Images

Update the `post_list.html` template to display the uploaded images:

```html
<ul>
    {% for post in posts %}
        <li>
            <strong>{{ post.title }}</strong><br>
            {% if post.image %}
                <img src="{{ post.image.url }}" alt="{{ post.title }}" style="max-width: 200px;"><br>  <!-- Display the image -->
            {% endif %}
            {{ post.content|truncatewords:20 }}
            <a href="#">Read more</a>
        </li>
    {% empty %}
        <li>No posts available.</li>
    {% endfor %}
</ul>
```

### Step 5: Serving Media Files in Development

To serve media files during development, you need to add a URL pattern in your project’s `urls.py`:

```python
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('myapp.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)  # Serve media files
```

### Step 6: Testing File Uploads

Run the development server again:

```bash
python manage.py runserver
```

Navigate to `http://127.0.0.1:8000/`, and try uploading an image along with a blog post. You should see the uploaded image displayed alongside the post.

## Conclusion

In this lesson, you learned how to manage static files and user-uploaded media in Django. You configured static file settings, created a form for file uploads, and displayed uploaded images on your web pages. With this knowledge, you can enhance your Django applications by incorporating styles, scripts, and user-generated content effectively. In the next lesson, we will explore advanced features of Django forms, including file uploads and formsets.