# Lesson 9: Building Your First API

In this lesson, we will build your first API using Django REST Framework (DRF). We will create API endpoints that allow you to perform CRUD (Create, Read, Update, Delete) operations on your models. By the end of this lesson, you will have a fully functional API for managing blog posts.

## Creating API Endpoints Using Serializers

### Step 1: Review Your Serializer

Before we create the API endpoints, let’s ensure that we have a serializer set up for our `Post` model. If you haven't already created a serializer, open `serializers.py` in your app directory (`myapp/serializers.py`) and ensure it looks like this:

```python
from rest_framework import serializers
from .models import Post

class PostSerializer(serializers.ModelSerializer):
    class Meta:
        model = Post
        fields = ['id', 'title', 'content', 'created_at', 'updated_at', 'image']  # Specify fields to serialize
```

This serializer will be used to convert `Post` instances to JSON format and validate incoming data.

### Step 2: Create API Views

Next, we will create views that correspond to the CRUD operations. Open `views.py` in your app directory (`myapp/views.py`) and add the following code:

```python
from rest_framework import generics
from .models import Post
from .serializers import PostSerializer

class PostList(generics.ListCreateAPIView):
    queryset = Post.objects.all()  # Retrieve all posts
    serializer_class = PostSerializer  # Specify the serializer to use

class PostDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Post.objects.all()  # Retrieve all posts
    serializer_class = PostSerializer  # Specify the serializer to use
```

### Explanation of the Views

- **`PostList`**: This view handles both GET and POST requests. It retrieves a list of all posts and allows the creation of new posts.

- **`PostDetail`**: This view handles GET, PUT, and DELETE requests for a specific post identified by its ID. It allows you to retrieve, update, or delete a post.

## Implementing CRUD Operations for Your Models

### Step 3: Set Up URL Routing for the API

To connect your API views to URLs, you need to define the URL patterns. If you haven't already created an `api.py` file in your app directory (`myapp/api.py`), create it and add the following code:

```python
from django.urls import path
from .views import PostList, PostDetail

urlpatterns = [
    path('posts/', PostList.as_view(), name='post-list'),  # URL for listing and creating posts
    path('posts/<int:pk>/', PostDetail.as_view(), name='post-detail'),  # URL for retrieving, updating, and deleting a post
]
```

### Step 4: Include API URLs in the Project

Now, include the API URLs in your project's main `urls.py` file (`myfirstproject/urls.py`):

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('myapp.api')),  # Include the API URLs
]
```

### Step 5: Testing the API Endpoints

Run the development server:

```bash
python manage.py runserver
```

You can test your API using a tool like Postman or cURL. Here are the CRUD operations you can perform:

1. **Create a New Post** (POST request):

   ```
   POST http://127.0.0.1:8000/api/posts/
   Content-Type: application/json

   {
       "title": "My New Post",
       "content": "This is the content of my new post.",
       "image": null  // Include an image URL if applicable
   }
   ```

   - This request creates a new post with the specified title and content.

2. **Retrieve All Posts** (GET request):

   ```
   GET http://127.0.0.1:8000/api/posts/
   ```

   - This request retrieves a list of all posts in JSON format.

3. **Retrieve a Specific Post** (GET request):

   ```
   GET http://127.0.0.1:8000/api/posts/1/
   ```

   - Replace `1` with the ID of the post you want to retrieve.

4. **Update a Post** (PUT request):

   ```
   PUT http://127.0.0.1:8000/api/posts/1/
   Content-Type: application/json

   {
       "title": "Updated Post Title",
       "content": "Updated content."
   }
   ```

   - This request updates the post with the specified ID.

5. **Delete a Post** (DELETE request):

   ```
   DELETE http://127.0.0.1:8000/api/posts/1/
   ```

   - This request deletes the post with the specified ID.

### Step 6: Handling Image Uploads

If your `Post` model includes an image field, you can handle image uploads by modifying the `PostList` view to accept file uploads. Update the `PostList` view in `views.py`:

```python
class PostList(generics.ListCreateAPIView):
    queryset = Post.objects.all()
    serializer_class = PostSerializer

    def perform_create(self, serializer):
        serializer.save()  # Save the new post instance
```

### Testing Image Uploads

To test image uploads, use Postman to send a POST request with form-data:

```
POST http://127.0.0.1:8000/api/posts/
```

In the body, select `form-data` and add the fields:

- **Key**: `title`, **Value**: `My New Post`
- **Key**: `content`, **Value**: `This is the content of my new post.`
- **Key**: `image`, **Value**: (select an image file)

## Conclusion

In this lesson, you learned how to build your first API using Django REST Framework. You created API endpoints for performing CRUD operations on your `Post` model, allowing you to create, read, update, and delete posts through HTTP requests. With this foundation, you can now expand your API to include more features and endpoints as needed. In the next lesson, we will explore advanced features of Django REST Framework, including authentication and permissions.