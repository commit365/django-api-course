# Lesson 12: Deployment Basics

In this lesson, we will cover the essential steps to prepare your Django application for production deployment. We will also provide an overview of popular deployment options, including Heroku and AWS, to help you choose the best platform for your application.

## Preparing Your Django Application for Production

### Step 1: Update Settings for Production

Before deploying your Django application, you need to make several changes to your `settings.py` file to ensure it is secure and optimized for production.

1. **Set `DEBUG` to `False`**: In production, you should set the `DEBUG` setting to `False` to prevent detailed error messages from being displayed to users.

   ```python
   DEBUG = False
   ```

2. **Configure Allowed Hosts**: You must specify the domains or IP addresses that your Django application can serve. Add your domain name or IP address to the `ALLOWED_HOSTS` list.

   ```python
   ALLOWED_HOSTS = ['yourdomain.com', 'www.yourdomain.com', 'your_ip_address']
   ```

3. **Set Up Static and Media Files**: Ensure that your static and media files are correctly configured. Set the `STATIC_ROOT` and `MEDIA_ROOT` settings to define where these files will be collected.

   ```python
   STATIC_URL = '/static/'
   STATIC_ROOT = BASE_DIR / 'staticfiles'  # Directory to collect static files

   MEDIA_URL = '/media/'
   MEDIA_ROOT = BASE_DIR / 'media'  # Directory to store uploaded media files
   ```

4. **Use a Secure Secret Key**: In production, you should use a secure and unique secret key. Avoid hardcoding it in your `settings.py`. Instead, consider using environment variables or a separate configuration file.

   ```python
   import os

   SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY', 'your_default_secret_key')
   ```

### Step 2: Collect Static Files

Before deploying, you need to collect all static files into the directory specified by `STATIC_ROOT`. Run the following command:

```bash
python manage.py collectstatic
```

This command collects static files from each app and places them in the `STATIC_ROOT` directory.

### Step 3: Configure a Database for Production

In production, you should use a robust database system such as PostgreSQL or MySQL instead of the default SQLite database. Update your `DATABASES` setting in `settings.py` to connect to your production database.

For example, to configure PostgreSQL:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'your_db_name',
        'USER': 'your_db_user',
        'PASSWORD': 'your_db_password',
        'HOST': 'localhost',  # Or your database server address
        'PORT': '5432',       # Default PostgreSQL port
    }
}
```

### Step 4: Set Up Security Measures

1. **Use HTTPS**: Ensure your application is served over HTTPS to encrypt data transmitted between the client and server. You can obtain an SSL certificate from providers like Let's Encrypt.

2. **Configure Security Middleware**: Enable Django’s security middleware to protect against common vulnerabilities. Add the following middleware to your `MIDDLEWARE` setting:

   ```python
   MIDDLEWARE = [
       ...
       'django.middleware.security.SecurityMiddleware',
       ...
   ]
   ```

3. **Set Security Headers**: You can use the `django-secure` package to help add security headers to your responses. Install it via pip:

   ```bash
   pip install django-secure
   ```

   Then, add it to your middleware:

   ```python
   MIDDLEWARE = [
       ...
       'django.middleware.security.SecurityMiddleware',
       ...
       'djangosecure.middleware.SecurityMiddleware',
   ]
   ```

## Overview of Deployment Options

Once your application is prepared for production, you can choose from several deployment options. Here are some popular platforms:

### 1. Heroku

Heroku is a cloud platform that allows you to deploy and manage applications easily. It is beginner-friendly and provides a free tier for small projects.

**Key Features**:
- Simple deployment using Git.
- Automatic scaling and load balancing.
- Add-ons for databases, caching, and monitoring.

**Deployment Steps**:
1. Create a Heroku account and install the Heroku CLI.
2. Create a new Heroku app:
   ```bash
   heroku create your-app-name
   ```
3. Set environment variables for your secret key and database settings:
   ```bash
   heroku config:set DJANGO_SECRET_KEY='your_secret_key'
   ```
4. Push your code to Heroku:
   ```bash
   git push heroku main
   ```
5. Run migrations:
   ```bash
   heroku run python manage.py migrate
   ```

### 2. AWS (Amazon Web Services)

AWS is a comprehensive cloud computing platform that provides a wide range of services for deploying applications. It offers more control and flexibility but can be more complex to set up.

**Key Features**:
- Scalable infrastructure with services like EC2, RDS, and S3.
- Pay-as-you-go pricing model.
- Extensive documentation and community support.

**Deployment Steps**:
1. Create an AWS account and set up an EC2 instance.
2. Install and configure a web server (e.g., Nginx or Apache) and a WSGI server (e.g., Gunicorn).
3. Set up a PostgreSQL or MySQL database using RDS.
4. Configure your Django application to connect to the database.
5. Use S3 for serving static and media files.

### 3. DigitalOcean

DigitalOcean is a cloud infrastructure provider that offers virtual private servers (droplets) for deploying applications. It is known for its simplicity and user-friendly interface.

**Key Features**:
- Affordable pricing with predictable billing.
- One-click applications for quick setup.
- Community tutorials and resources.

**Deployment Steps**:
1. Create a DigitalOcean account and create a droplet.
2. SSH into your droplet and install the necessary software (Python, Django, PostgreSQL, Nginx).
3. Clone your project repository and set up your environment.
4. Configure Nginx to serve your Django application.

### 4. PythonAnywhere

PythonAnywhere is a cloud-based platform specifically designed for hosting Python applications. It is beginner-friendly and suitable for small to medium-sized projects.

**Key Features**:
- Easy setup for Python applications.
- Built-in support for web frameworks like Django.
- Free tier available for small projects.

**Deployment Steps**:
1. Create a PythonAnywhere account.
2. Set up a new web app and choose Django as the framework.
3. Upload your project files and configure the web app settings.
4. Set environment variables for your application.

## Conclusion

In this lesson, you learned how to prepare your Django application for production deployment. You explored essential settings, security measures, and the process of collecting static files. Additionally, you gained an overview of popular deployment options, including Heroku, AWS, DigitalOcean, and PythonAnywhere. With this knowledge, you can confidently deploy your Django applications and make them accessible to users. In the next lesson, we will explore advanced deployment techniques and best practices for maintaining your application in production.