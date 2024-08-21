# Lesson 10: Authentication and Permissions

In this lesson, we will explore how to implement authentication and permissions in your Django REST Framework (DRF) APIs. We will cover the different authentication methods available in Django and demonstrate how to set up token-based authentication for your API.

## Understanding Authentication Methods in Django

Authentication is the process of verifying the identity of a user or client. Django provides several built-in authentication methods, including:

1. **Session Authentication**: This is the default authentication method in Django. It uses sessions to keep track of authenticated users. When a user logs in, a session is created, and the user is identified by a session ID stored in a cookie.

2. **Basic Authentication**: This method uses HTTP Basic Authentication, where the client sends a username and password with each request. The credentials are base64 encoded and included in the request header.

3. **Token Authentication**: This method uses tokens to authenticate users. When a user logs in, they receive a token that can be used for subsequent requests. This is particularly useful for APIs, as it allows for stateless authentication.

4. **OAuth2**: This is a more complex authentication method that allows third-party applications to access user data without exposing user credentials. It is commonly used for social logins and integrations.

## Implementing Token-Based Authentication for APIs

### Step 1: Install Django REST Framework and Django REST Framework Authtoken

To use token-based authentication, you need to install the `djangorestframework` and `djangorestframework-authtoken` packages. If you haven't already installed them, run the following command:

```bash
pip install djangorestframework djangorestframework-authtoken
```

### Step 2: Update `settings.py`

After installing the required packages, you need to add `rest_framework.authtoken` to your `INSTALLED_APPS` in the `settings.py` file:

```python
INSTALLED_APPS = [
    ...
    'rest_framework',
    'rest_framework.authtoken',  # Add token authentication app
    'myapp',
]
```

### Step 3: Create the Token Model

Next, you need to create the token table in your database. Run the following command to create the necessary migrations:

```bash
python manage.py migrate
```

### Step 4: Create a User Registration View

To allow users to register and obtain a token, create a view for user registration. Open `views.py` in your app directory (`myapp/views.py`) and add the following code:

```python
from django.contrib.auth.models import User
from rest_framework import generics
from rest_framework.response import Response
from rest_framework.authtoken.models import Token
from rest_framework import status
from .serializers import UserSerializer  # Create this serializer

class UserRegistration(generics.CreateAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer

    def create(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        user = serializer.save()
        token, created = Token.objects.get_or_create(user=user)
        return Response({'token': token.key}, status=status.HTTP_201_CREATED)
```

### Step 5: Create a User Serializer

Create a serializer for the user in a new file named `serializers.py` in your app directory (`myapp/serializers.py`):

```python
from django.contrib.auth.models import User
from rest_framework import serializers

class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ['username', 'password']
        extra_kwargs = {'password': {'write_only': True}}  # Password should be write-only

    def create(self, validated_data):
        user = User(**validated_data)
        user.set_password(validated_data['password'])  # Hash the password
        user.save()
        return user
```

### Step 6: Set Up URL Routing for Authentication

Add a URL pattern for the user registration view. Open `api.py` in your app directory (`myapp/api.py`) and add the following line:

```python
from .views import UserRegistration

urlpatterns = [
    path('posts/', PostList.as_view(), name='post-list'),
    path('posts/<int:pk>/', PostDetail.as_view(), name='post-detail'),
    path('register/', UserRegistration.as_view(), name='user-registration'),  # Add this line
]
```

### Step 7: Configure Token Authentication

To enable token authentication for your API views, you need to update the `settings.py` file. Add the following configuration:

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.TokenAuthentication',  # Use token authentication
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',  # Require authentication by default
    ],
}
```

### Step 8: Testing User Registration and Token Authentication

Run the development server:

```bash
python manage.py runserver
```

You can test the user registration and token authentication using Postman or cURL.

1. **Register a New User** (POST request):

   ```
   POST http://127.0.0.1:8000/api/register/
   Content-Type: application/json

   {
       "username": "testuser",
       "password": "testpassword"
   }
   ```

   - This request creates a new user and returns a token.

2. **Access Protected Endpoints** (GET request):

   To access protected endpoints, include the token in the request headers. For example, to retrieve all posts:

   ```
   GET http://127.0.0.1:8000/api/posts/
   Authorization: Token your_token_here
   ```

   - Replace `your_token_here` with the token you received during registration.

## Conclusion

In this lesson, you learned how to implement authentication and permissions in your Django REST Framework API. You set up token-based authentication, created a user registration view, and configured your API to require authentication for protected endpoints. With this foundation, you can now secure your API and manage user access effectively. In the next lesson, we will explore advanced features such as permissions and throttling in Django REST Framework.