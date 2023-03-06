==ab==

# My_First_Django_RESTAPI
To use Django REST framework, Django project has to be created

### Step 0: Install Django and djangorestframework with pip

    $ python3 -m pip install Django djangorestframework
    
### Step 1: Create a new project "countryapi" by using django-admin tool

    $ django-admin startproject countryapi
This command also creates a new folder in your current directory with the same name. This folder contains configurations and settings for the project

### Step 2: Create the application inside the project folder (change directory)
    
    $ python3 manage.py startapp countries
This command creates a folder with same name of application that has bas files for the application. Make sure the directory is changed to countryapi 

### Step 3: Config settings.py (config file)

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

### Step 4: Define fields of data (2 steps) / Django migration
First step, edit the models.py. Same as making fields in SQL ({'name': acbcd, 'capital': ABCD, 'area': 100})


    # countries/models.py
    from django.db import models

    class Country(models.Model):
        name = models.CharField(max_length=100)
        capital = models.CharField(max_length=100)
        area = models.IntegerField(help_text="(in square kilometers)")

Second step, Django migration. Model class in python file does not create database table in the database. Migration does, new change to model -> change to database.

![Migrate example](https://files.realpython.com/media/model_to_schema.4e4b8506dc26.png)

    $ python3 manage.py makemigrations
    Migrations for 'countries':
      countries/migrations/0001_initial.py
        - Create model Country

    $ python3 manage.py migrate
    Operations to perform:
      Apply all migrations: admin, auth, contenttypes, countries, sessions
    Running migrations:
      Applying contenttypes.0001_initial... OK
      Applying auth.0001_initial... OK
      ...

"Django migrations" to create a new table in the database. Then using a Django fixture to load some data in the database.
<br> Create a JSON file <countries.json> and save it in countries directory

    $ python3 manage.py loaddata countries.json
    Installed 3 object(s) from 1 fixture(s)
    
### Step 5: Django fixture - Put some initial datas in the table
Create a "abc.json" and put it in the application directory

    $ python3 manage.py loaddata countries.json <- full absolute path
    Installed 3 object(s) from 1 fixture(s)

### Step 6: Serialization
Takes an existing Django model and converts it to JSON for a REST API -> Done by model serializers: tell DRF how to convert a model instance into JSON and what data to include.
<br> Create a file called <serializers.py> in main application and add the below code:

    # countries/serializers.py
    from rest_framework import serializers
    from .models import Country

    class CountrySerializer(serializers.ModelSerializer):
        class Meta:
            model = Country
            fields = ["id", "name", "capital", "area"]
            
### Step 7: View/query data **

Just like Django, Django REST framework uses "view" to query data from the database to display to the user.
<br> No need to write from sratch, we can subclass DRF's ModelViewSet class, which has default views for common REST API operations.

    # countries/views.py
    from rest_framework import viewsets

    from .models import Country
    from .serializers import CountrySerializer

    class CountryViewSet(viewsets.ModelViewSet):
        serializer_class = CountrySerializer
        queryset = Country.objects.all()
       
### Step 8: Map URL 

Once the views are created, they need to be mapped to the appropriate URLs or endpoints. To do this, Django REST framework provides a DefaultRouter that will automatically generate URLs for a ModelViewSet.

    # countries/urls.py
    from django.urls import path, include
    from rest_framework.routers import DefaultRouter

    from .views import CountryViewSet

    router = DefaultRouter()
    router.register(r"countries", CountryViewSet)

    urlpatterns = [
        path("", include(router.urls))
    ]
    
### Step 9: Update project's based urls.py to include all the countries URLs in the project
In countryapi (config folder)

    # countryapi/urls.py
    from django.contrib import admin
    from django.urls import path, include

    urlpatterns = [
        path("admin/", admin.site.urls),
        path("", include("countries.urls")),
    ]
       
### Step 10: Run

    $ python3 manage.py runserver
    
The server is running

    $ curl -i http://127.0.0.1:8000/countries/ -w '\n'

   

