# Lesson 13: Integrating Third-Party APIs

In this lesson, we will learn how to integrate third-party APIs into your Django application. We will cover how to make HTTP requests to external APIs, handle responses, and integrate the retrieved data into your app. This is a crucial skill for enhancing your application with external data and services.

## Making HTTP Requests to External APIs

### Step 1: Install the `requests` Library

To make HTTP requests in Python, we will use the popular `requests` library. If you haven't already installed it, you can do so using pip:

```bash
pip install requests
```

### Step 2: Making a GET Request

Let’s start by making a simple GET request to an external API. For this example, we will use the JSONPlaceholder API, a free fake online REST API for testing and prototyping.

Create a new function in your views file (`myapp/views.py`) to fetch data from the API:

```python
import requests
from django.http import JsonResponse
from django.views import View

class ExternalAPIView(View):
    def get(self, request):
        url = 'https://jsonplaceholder.typicode.com/posts'  # URL of the external API
        response = requests.get(url)  # Make the GET request

        if response.status_code == 200:  # Check if the request was successful
            data = response.json()  # Parse the JSON response
            return JsonResponse(data, safe=False)  # Return the data as a JSON response
        else:
            return JsonResponse({'error': 'Failed to fetch data'}, status=response.status_code)
```

### Explanation of the Code

- **`requests.get(url)`**: This function makes a GET request to the specified URL and returns a response object.

- **`response.status_code`**: This attribute contains the HTTP status code returned by the server. A status code of 200 indicates a successful request.

- **`response.json()`**: This method parses the response content as JSON and returns it as a Python dictionary.

- **`JsonResponse(data, safe=False)`**: This function returns a JSON response to the client. The `safe=False` parameter allows the response to return a list instead of a dictionary.

### Step 3: Setting Up URL Routing for the External API View

Next, you need to add a URL pattern for the `ExternalAPIView`. Open `api.py` in your app directory (`myapp/api.py`) and add the following line:

```python
from .views import ExternalAPIView

urlpatterns = [
    path('posts/', PostList.as_view(), name='post-list'),
    path('posts/<int:pk>/', PostDetail.as_view(), name='post-detail'),
    path('external-api/', ExternalAPIView.as_view(), name='external-api'),  # Add this line
]
```

### Step 4: Testing the External API Integration

Run the development server:

```bash
python manage.py runserver
```

Open your web browser and navigate to `http://127.0.0.1:8000/api/external-api/`. You should see a JSON response containing a list of posts fetched from the JSONPlaceholder API.

## Handling Responses and Integrating Data into Your App

### Step 5: Processing the Response Data

You can further process the data received from the external API before integrating it into your application. For example, you might want to filter or transform the data.

Here’s an updated version of the `ExternalAPIView` that filters the results to return only posts with a specific user ID:

```python
class ExternalAPIView(View):
    def get(self, request):
        url = 'https://jsonplaceholder.typicode.com/posts'
        response = requests.get(url)

        if response.status_code == 200:
            data = response.json()
            # Filter posts by userId (for example, userId = 1)
            filtered_data = [post for post in data if post['userId'] == 1]
            return JsonResponse(filtered_data, safe=False)
        else:
            return JsonResponse({'error': 'Failed to fetch data'}, status=response.status_code)
```

### Step 6: Integrating Data into Your Application

Once you have the data from the external API, you can integrate it into your application. For example, you might want to display the posts on your website.

You can create a new template to render the posts. Create a new HTML file named `external_posts.html` in your `templates/myapp/` directory:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>External Posts</title>
</head>
<body>
    <h1>External Posts</h1>
    <ul id="post-list"></ul>

    <script>
        fetch('/api/external-api/')
            .then(response => response.json())
            .then(data => {
                const postList = document.getElementById('post-list');
                data.forEach(post => {
                    const li = document.createElement('li');
                    li.textContent = post.title;  // Display the post title
                    postList.appendChild(li);
                });
            })
            .catch(error => console.error('Error fetching posts:', error));
    </script>
</body>
</html>
```

### Step 7: Creating a View to Render the Template

Now, create a view to render the `external_posts.html` template. Add the following code to your `views.py`:

```python
class ExternalPostsView(View):
    def get(self, request):
        return render(request, 'myapp/external_posts.html')
```

### Step 8: Setting Up URL Routing for the External Posts View

Add a URL pattern for the `ExternalPostsView` in your `api.py`:

```python
from .views import ExternalPostsView

urlpatterns = [
    path('posts/', PostList.as_view(), name='post-list'),
    path('posts/<int:pk>/', PostDetail.as_view(), name='post-detail'),
    path('external-api/', ExternalAPIView.as_view(), name='external-api'),
    path('external-posts/', ExternalPostsView.as_view(), name='external-posts'),  # Add this line
]
```

### Step 9: Testing the Integration

Run the server again and navigate to `http://127.0.0.1:8000/api/external-posts/`. You should see a list of post titles fetched from the external API displayed on the page.

## Conclusion

In this lesson, you learned how to integrate third-party APIs into your Django application. You made HTTP requests to external APIs, handled responses, and integrated the retrieved data into your app. This capability allows you to enhance your application with external data and services, providing a richer experience for your users. In the next lesson, we will explore advanced topics such as caching API responses and error handling for API integrations.