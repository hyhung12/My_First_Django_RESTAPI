==ab==

# My_First_Django_RESTAPI
To use Django REST framework, Django project has to be created

### Step 1: Install Django and djangorestframework with pip:

    $ python -m pip install Django djangorestframework
    
### Step 2: Project "countryapi" is created by using django-admin tool

    $ django-admin startproject countryapi
This command also creates a new folder in your current directory with the same name. This folder contains configurations and settings for the project

### Step 3: Create the application named countries
    
    $ python manage.py startapp countries
This command creates a folder with same name of application that has bas files for the application

### Step 4: Config settings.py (config file)

    # countryapi/settings.py
    INSTALLED_APPS = [
        "django.contrib.admin",
        "django.contrib.auth",
        "django.contrib.contenttypes",
        "django.contrib.sessions",
        "django.contrib.messages",
        "django.contrib.staticfiles",
        ->"rest_framework",<-
        ->"countries",<-
    ]
Django REST framework is part of Django. Django is just a framework designed for web development and DRF is the library used in Django to build RESTful APIs.

### Step 5: Define fields of data (in models.py)

    # countries/models.py
    from django.db import models

    class Country(models.Model):
        name = models.CharField(max_length=100)
        capital = models.CharField(max_length=100)
        area = models.IntegerField(help_text="(in square kilometers)")

This code defines a Country model. Django will use this model to create the database table and columns for the country data. Then run 2 below commands to update the database based on model

    $ python manage.py makemigrations
    Migrations for 'countries':
      countries/migrations/0001_initial.py
        - Create model Country

    $ python manage.py migrate
    Operations to perform:
      Apply all migrations: admin, auth, contenttypes, countries, sessions
    Running migrations:
      Applying contenttypes.0001_initial... OK
      Applying auth.0001_initial... OK
      ...


    
   

