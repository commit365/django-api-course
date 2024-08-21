# Lesson 21: Final Project: Building a Full-Stack Application

In this lesson, we will guide you through the process of building a full-stack application by combining Django with a front-end framework, such as React. We will also cover how to deploy your full-stack application to a live server. This project will help you integrate your knowledge of Django with modern front-end technologies.

## Combining Django with a Front-End Framework (e.g., React)

### Step 1: Setting Up the Django Backend

1. **Create a New Django Project**: If you haven’t already, create a new Django project and app. For example, let’s create a project called `myfullstackapp` and an app called `api`.

   ```bash
   django-admin startproject myfullstackapp
   cd myfullstackapp
   python manage.py startapp api
   ```

2. **Set Up the Django REST Framework**: Install Django REST Framework if you haven’t done so already:

   ```bash
   pip install djangorestframework
   ```

3. **Configure Your Project**: Add `rest_framework` and your app (`api`) to the `INSTALLED_APPS` in `settings.py`:

   ```python
   INSTALLED_APPS = [
       ...
       'rest_framework',
       'api',
   ]
   ```

4. **Create Your Models**: Define your models in `api/models.py`. For example, let’s create a simple `Post` model:

   ```python
   from django.db import models

   class Post(models.Model):
       title = models.CharField(max_length=200)
       content = models.TextField()
       created_at = models.DateTimeField(auto_now_add=True)

       def __str__(self):
           return self.title
   ```

5. **Create Serializers**: In `api/serializers.py`, create a serializer for your model:

   ```python
   from rest_framework import serializers
   from .models import Post

   class PostSerializer(serializers.ModelSerializer):
       class Meta:
           model = Post
           fields = '__all__'
   ```

6. **Create API Views**: In `api/views.py`, create views to handle API requests:

   ```python
   from rest_framework import generics
   from .models import Post
   from .serializers import PostSerializer

   class PostList(generics.ListCreateAPIView):
       queryset = Post.objects.all()
       serializer_class = PostSerializer

   class PostDetail(generics.RetrieveUpdateDestroyAPIView):
       queryset = Post.objects.all()
       serializer_class = PostSerializer
   ```

7. **Set Up URL Routing**: In `api/urls.py`, define the URL patterns for your API:

   ```python
   from django.urls import path
   from .views import PostList, PostDetail

   urlpatterns = [
       path('posts/', PostList.as_view(), name='post-list'),
       path('posts/<int:pk>/', PostDetail.as_view(), name='post-detail'),
   ]
   ```

   Include these URLs in your project’s `urls.py`:

   ```python
   from django.contrib import admin
   from django.urls import path, include

   urlpatterns = [
       path('admin/', admin.site.urls),
       path('api/', include('api.urls')),  # Include your API URLs
   ]
   ```

8. **Migrate the Database**: Run migrations to create the database tables:

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

### Step 2: Setting Up the React Front-End

1. **Create a React App**: Use Create React App to set up a new front-end project. Navigate to the directory where you want to create your React app and run:

   ```bash
   npx create-react-app frontend
   cd frontend
   ```

2. **Install Axios**: Install Axios for making HTTP requests to your Django API:

   ```bash
   npm install axios
   ```

3. **Create Components**: Create a component to fetch and display posts. In `src/components`, create a file named `PostList.js`:

   ```javascript
   import React, { useEffect, useState } from 'react';
   import axios from 'axios';

   const PostList = () => {
       const [posts, setPosts] = useState([]);

       useEffect(() => {
           const fetchPosts = async () => {
               const response = await axios.get('http://127.0.0.1:8000/api/posts/');
               setPosts(response.data);
           };
           fetchPosts();
       }, []);

       return (
           <div>
               <h1>Posts</h1>
               <ul>
                   {posts.map(post => (
                       <li key={post.id}>{post.title}</li>
                   ))}
               </ul>
           </div>
       );
   };

   export default PostList;
   ```

4. **Update `App.js`**: Import and use the `PostList` component in your `src/App.js`:

   ```javascript
   import React from 'react';
   import PostList from './components/PostList';

   const App = () => {
       return (
           <div className="App">
               <PostList />
           </div>
       );
   };

   export default App;
   ```

5. **Run the React App**: Start your React development server:

   ```bash
   npm start
   ```

### Step 3: Connecting the Front-End and Back-End

1. **CORS Configuration**: To allow your React app to communicate with your Django API, you need to enable Cross-Origin Resource Sharing (CORS). Install the `django-cors-headers` package:

   ```bash
   pip install django-cors-headers
   ```

   Add it to your `INSTALLED_APPS` and configure it in `settings.py`:

   ```python
   INSTALLED_APPS = [
       ...
       'corsheaders',
   ]

   MIDDLEWARE = [
       ...
       'corsheaders.middleware.CorsMiddleware',
       ...
   ]

   CORS_ALLOWED_ORIGINS = [
       "http://localhost:3000",  # React app running on port 3000
   ]
   ```

2. **Test the Application**: Ensure both the Django server and the React app are running. Navigate to `http://localhost:3000` to see your React app displaying posts fetched from the Django API.

## Deploying Your Full-Stack Application to a Live Server

### Step 4: Preparing the Django Backend for Deployment

1. **Update Settings for Production**: Ensure your `settings.py` is configured for production, including setting `DEBUG` to `False`, configuring `ALLOWED_HOSTS`, and setting up database configurations.

2. **Collect Static Files**: Run the following command to collect static files:

   ```bash
   python manage.py collectstatic
   ```

3. **Set Up a Production Database**: Use a production-grade database like PostgreSQL or MySQL. Update your `DATABASES` setting accordingly.

### Step 5: Deploying the Django Application

1. **Choose a Hosting Service**: You can deploy your Django application on platforms like Heroku, DigitalOcean, or AWS. For this example, we will use Heroku.

2. **Install the Heroku CLI**: If you haven’t already, install the Heroku CLI from the [Heroku website](https://devcenter.heroku.com/articles/heroku-cli).

3. **Create a Heroku App**: In your terminal, navigate to your Django project directory and run:

   ```bash
   heroku create your-app-name
   ```

4. **Push Your Code to Heroku**: Make sure your code is committed to Git, then push it to Heroku:

   ```bash
   git push heroku main
   ```

5. **Run Migrations on Heroku**: After deploying, run your migrations:

   ```bash
   heroku run python manage.py migrate
   ```

6. **Set Environment Variables**: Set your environment variables, including your secret key and database settings:

   ```bash
   heroku config:set DJANGO_SECRET_KEY='your_secret_key'
   ```

### Step 6: Deploying the React Front-End

1. **Build Your React App**: In your React app directory, run the build command:

   ```bash
   npm run build
   ```

   This creates a `build` directory with the production-ready files.

2. **Serve React with Django**: You can serve the React app using Django by placing the contents of the `build` directory in your Django static files directory or using a dedicated web server like Nginx.

3. **Deploy to a Hosting Service**: Alternatively, you can deploy your React app separately on platforms like Vercel, Netlify, or Heroku.

### Step 7: Testing the Live Application

After deploying both your Django backend and React frontend, navigate to your live application URL to ensure everything is functioning correctly. Test the API endpoints and ensure that the React app can fetch and display data as expected.

## Conclusion

In this lesson, you learned how to build a full-stack application by combining Django with a front-end framework like React. You set up a Django backend to serve an API and created a React frontend to consume that API. Additionally, you learned how to deploy your full-stack application to a live server, making it accessible to users. This project integrates all the concepts you’ve learned throughout the course, providing a solid foundation for building modern web applications. In the next lesson, we will explore advanced topics and features to further enhance your skills in Django development.