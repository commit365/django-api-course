# Lesson 14: Advanced API Features

In this lesson, we will explore advanced features of APIs using Django REST Framework (DRF). Specifically, we will cover how to implement pagination and filtering in your APIs, as well as how to use webhooks for real-time data updates. These features will enhance the usability and responsiveness of your API.

## Implementing Pagination and Filtering in APIs

### Step 1: Setting Up Pagination

Pagination allows you to split large datasets into smaller, manageable chunks. DRF provides built-in pagination classes that you can easily integrate into your API.

1. **Update `settings.py`**: Open your `settings.py` file and add the following configuration to enable pagination:

   ```python
   REST_FRAMEWORK = {
       'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
       'PAGE_SIZE': 10,  # Number of items per page
   }
   ```

   This configuration sets the default pagination class to `PageNumberPagination`, which uses page numbers to navigate through the results.

### Step 2: Testing Pagination in Your API

Now, when you access your `PostList` API endpoint, it will automatically paginate the results. You can test the pagination by making a GET request to the posts endpoint:

```
GET http://127.0.0.1:8000/api/posts/?page=1
```

The response will include metadata about the pagination, such as the total number of pages and the number of items on the current page.

### Step 3: Implementing Filtering

Filtering allows clients to retrieve specific subsets of data based on query parameters. DRF provides a powerful filtering system that can be easily integrated.

1. **Install Django Filter**: First, install the `django-filter` package:

   ```bash
   pip install django-filter
   ```

2. **Update `settings.py`**: Add `django_filters` to your `INSTALLED_APPS` in `settings.py`:

   ```python
   INSTALLED_APPS = [
       ...
       'django_filters',
   ]
   ```

3. **Update Your View for Filtering**: Modify the `PostList` view in `views.py` to enable filtering:

   ```python
   from django_filters.rest_framework import DjangoFilterBackend
   from rest_framework import filters

   class PostList(generics.ListCreateAPIView):
       queryset = Post.objects.all()
       serializer_class = PostSerializer
       filter_backends = [DjangoFilterBackend, filters.OrderingFilter]
       filterset_fields = ['title', 'created_at']  # Fields to filter by
       ordering_fields = ['created_at', 'title']  # Fields to order by
       ordering = ['created_at']  # Default ordering
   ```

### Step 4: Testing Filtering in Your API

You can now filter the posts by title or creation date. For example, to filter posts by title, you can make a GET request like this:

```
GET http://127.0.0.1:8000/api/posts/?title=My%20New%20Post
```

To order the results by creation date, you can add an ordering parameter:

```
GET http://127.0.0.1:8000/api/posts/?ordering=created_at
```

## Using Webhooks for Real-Time Data Updates

Webhooks allow you to receive real-time updates from external services. When a specific event occurs in a third-party service, it sends an HTTP POST request to a specified URL in your application. This is useful for integrating with services that provide real-time data, such as payment gateways or notification systems.

### Step 1: Setting Up a Webhook Endpoint

1. **Create a New View for the Webhook**: In your `views.py`, create a new view to handle incoming webhook requests:

   ```python
   from django.views.decorators.csrf import csrf_exempt
   from django.http import JsonResponse
   import json

   @csrf_exempt  # Disable CSRF protection for this view
   def webhook(request):
       if request.method == 'POST':
           data = json.loads(request.body)  # Parse the JSON data from the request
           # Process the data (e.g., update your database, send notifications, etc.)
           print(data)  # For demonstration purposes, print the data to the console
           return JsonResponse({'status': 'success'}, status=200)
       return JsonResponse({'error': 'Invalid request'}, status=400)
   ```

### Step 2: Setting Up URL Routing for the Webhook

Add a URL pattern for the webhook in your `api.py`:

```python
from .views import webhook

urlpatterns = [
    path('posts/', PostList.as_view(), name='post-list'),
    path('posts/<int:pk>/', PostDetail.as_view(), name='post-detail'),
    path('external-api/', ExternalAPIView.as_view(), name='external-api'),
    path('external-posts/', ExternalPostsView.as_view(), name='external-posts'),
    path('webhook/', webhook, name='webhook'),  # Add this line
]
```

### Step 3: Testing the Webhook

To test the webhook, you can use a tool like Postman to send a POST request to your webhook endpoint:

```
POST http://127.0.0.1:8000/api/webhook/
Content-Type: application/json

{
    "event": "new_post",
    "data": {
        "title": "Webhook Test Post",
        "content": "This post was created via a webhook."
    }
}
```

You should see the data printed in the console, and the response should indicate success.

### Step 4: Integrating with External Services

You can integrate your webhook with external services by providing them with the URL of your webhook endpoint. For example, if you are using a payment gateway, you can configure it to send notifications to your webhook URL whenever a payment is completed.

## Conclusion

In this lesson, you learned how to implement advanced features in your Django REST Framework API, including pagination and filtering for better data management. You also explored how to set up webhooks to receive real-time updates from external services. These features will enhance the functionality and responsiveness of your API, allowing for a better user experience. In the next lesson, we will explore best practices for API design and documentation.