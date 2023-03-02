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

### Step 4:Config settings.py (config file)
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


    
   

