<h1 align="center"><b>API-Development-in-Python-Django-with-Swagger</b></h1>

![API-img](./doc/API.png)

## Pre Requirements

| Tools  | Download Link  |
| ------ | ------ |
| python | [python](https://www.python.org/) |
| xampp  | [xampp](https://www.apachefriends.org/index.html) |
## Project Setup

    pip install virtualenv
    virtualenv env
    env\Scripts\activate
    pip install django
    pip install django-rest-framework
    pip install drf-yasg[validation]
    pip install django-cors-headers
    pip freeze > requirements.txt
    django-admin startproject server_config .
    python manage.py startapp api

### _Add the following line in the settings.py file_ (server_config folder)

    INSTALLED_APPS = [
        'corsheaders',
        'rest_framework',
        'drf_yasg',
    ]

    MIDDLEWARE = [
        'corsheaders.middleware.CorsMiddleware',
    ]

    REST_FRAMEWORK = {
        'DEFAULT_PERMISSION_CLASSES': (
            'rest_framework.permissions.AllowAny',
        )
    }

### _Add the following line in the urls.py file_ (server_config folder)

    from django import urls
    from django.contrib import admin
    from django.urls import path
    from django.urls.conf import include

    from rest_framework import permissions
    from drf_yasg.views import get_schema_view
    from drf_yasg import openapi

    schema_view = get_schema_view(
    openapi.Info(
        title="API_NAME",
        default_version='v3',
        description="Description",
        terms_of_service="URL",
        contact=openapi.Contact(email="EMAIL_ID"),
        license=openapi.License(name="BSD License"),
    ),
    public=True,
    permission_classes=(permissions.AllowAny,),
    )

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('api/',include('api.urls')),
        path('', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
        path('doc/', schema_view.with_ui('redoc', cache_timeout=0), name='schema-redoc'),
    ]



### _Add 2 Files in the api folder_ (api folder)

    urls.py
    serializers.py


### _Add the following line in the models.py file_ (api folder)

    from django.db import models

    class Basetable(models.Model):
        id = models.AutoField(primary_key=True)
        productname = models.CharField(max_length=100)
        description = models.CharField(max_length=100)

### _Add the following line in the serializers.py file_ (api folder)

    from django.db.models import fields
    from rest_framework import serializers
    from .models import *

    class BasetableSerializers(serializers.ModelSerializer):
        class Meta:
            model = Basetable
            fields = '__all__'


### _Add the following line in the views.py file_ (api folder)

    from django.shortcuts import render
    from .models import *
    from .serializers import *
    from rest_framework import mixins
    from rest_framework import generics


    class PostAPI(mixins.ListModelMixin,mixins.CreateModelMixin,generics.GenericAPIView):
        queryset = Basetable.objects.all()
        serializer_class = BasetableSerializers

        lookup_field = 'id_number'

        def get(sef, request, id_number = None):
            if id_number:
                return sef.retrieve(request)
            else:
                return sef.list(request)

### _Add the following line in the urls.py file_ (api folder)

    from django.urls import path
    from .views import *

    urlpatterns = [
        path('post/',PostAPI.as_view()),
    ]


### _if you change the default database to a MySQL database then add the following lines in the settings.py file_

```

'default': {
    'ENGINE'   : 'django.db.backends.mysql',
    'NAME'     : 'DB_NAME',
    'USER'     : 'DB_USER_NAME',
    'PASSWORD' : 'DB_PASS',
    'HOST'     : 'localhost',
    'PORT'     : '3306',
    }
    
```
    pip install mysqlclient

    python manage.py makemigrations
    python manage.py migrate
    python manage.py runserver

#

<h3 align="center">END</h3>

#

#### other commands
    python manage.py inspectdb


    pip install -r requirements.txt
