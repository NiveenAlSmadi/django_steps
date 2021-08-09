# django_steps
 ## Part A
 - mkdir `django_snacks`
 - cd `django_snacks`
 - `poetry init -n`
 - `poetry add django`
 - `poetry add --dev black`
 - `poetry shell`
 - django-admin startproject name_of_project .
 - `python manage.py runserver`
 - `python manage.py startapp name_of_app`
 - to run the test`python manage.py test`
____________________________________________
- in the `settings.py` ---> `INSTALLED_APPS` --->add the app to project applications `'snacks.apps.SnacksConfig',`

- in `project/urls.py`--->add `from django.urls import path, include`
- in `project/urls.py`---> `urlpatterns` ---> add `path('snacksapp/', include('snacks.urls')),`
- inside `snacks folder` create `urls.py`
____________________________________________
- in the root ~ create `templates`folder and make some `HTML` pages inside it
- go to `settings.py`--->add `import os`
- go to `settings.py`--->TEMPLATES--->add `'DIRS':[os.path.join(BASE_DIR, 'templates')],`



- in `snacks/views.py`:Add a template
```
from django.shortcuts import render
#Create home view
from django.views.generic import TemplateView

class HomeView(TemplateView):
    template_name = 'snacks-home.html'
```
- go to `snacks/urls.py` add :
```
from .views import HomeView
from django.urls import path

urlpatterns = [
    path('', HomeView.as_view(), name='home'), 
    
]
```

## Part B








## Part C
- `rm -rf nnnnn`

- `mkdir django-custom-user`
- `cd django-custom-user`
- `mkdir CustomUser`
- `cd CustomUser`
- `poetry init -n`
- `poetry add django`
- `poetry add --dev black`
- `poetry shell`
- `django-admin startproject custom_user_project .`
- `python manage.py startapp users`
-  `python manage.py runserver`&#x1F534;: DO NOT DO python manage.py migrate
_____________________________________________________________
go to `settings.py` ----> 
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'users.apps.UsersConfig', #or 'users'
]
AUTH_USER_MODEL = 'users.CustomUser'

```
go to `users/models.py` ----> 
```python
from django.db import models
from django.contrib.auth.models import AbstractUser
class CustomUser(AbstractUser):
    pass

    def __str__(self):
        return self.username
```

- create users/`forms.py`


- in forms.py write :
```python
from django import forms
from django.contrib.auth.forms import UserCreationForm, UserChangeForm
from .models import CustomUser

class CustomUserCreationForm(UserCreationForm):
    class Meta:
        model = CustomUser
        fields = ('username', 'email')

class CustomUserChangeForm(UserChangeForm):
    class Meta:
        model = CustomUser
        fields = ('username', 'email')
```

go to users/`admin.py` ----> add :
```python
from django.contrib import admin

from django.contrib.auth import get_user_model
from django.contrib.auth.admin import UserAdmin

from .forms import CustomUserCreationForm, CustomUserChangeForm
from .models import CustomUser

class CustomUserAdmin(UserAdmin):
    add_form = CustomUserCreationForm
    form = CustomUserChangeForm
    model = CustomUser
    list_display = ['email', 'username',]

admin.site.register(CustomUser, CustomUserAdmin)


```
in ubuntu ---->
- `python manage.py makemigrations users`
- `python manage.py migrate`
- `python manage.py createsuperuser`
- `python manage.py runserver`
______________________________________________
- go to `settings.py`----> `import os`
- go to `settings.py`---->`TEMPLATES`--->`'DIRS': [os.path.join(BASE_DIR, 'templates')],`
- go to `settings.py`----> 
```python
LOGIN_REDIRECT_URL = 'home'
LOGOUT_REDIRECT_URL = 'home'
```
_________________________________________________
- In the  root create folders `templates` & `templates/registration`
Create four (4) templates files :

    templates/registration/**login.html**

    templates/registration/**signup.html**

    templates/**base.html**

    templates/**home.html**


- update files with : 



    **base.html**

    ```
    <!-- templates/base.html -->
    <!DOCTYPE html>
    <html>
    <head>
    <meta charset="utf-8">
    <title>{% block title %}Django Auth Tutorial{% endblock %}</title>
    </head>
    <body>
    <main>
        {% block content %}
        {% endblock %}
    </main>
    </body>
    </html>
    ```

    **home.html**

    ```
    <!-- templates/home.html -->
    {% extends 'base.html' %}

    {% block title %}Home{% endblock %}

    {% block content %}
    {% if user.is_authenticated %}
    Hi {{ user.username }}!
    <p><a href="{% url 'logout' %}">Log Out</a></p>
    {% else %}
    <p>You are not logged in</p>
    <a href="{% url 'login' %}">Log In</a> |
    <a href="{% url 'signup' %}">Sign Up</a>
    {% endif %}
    {% endblock %}
    ```

    **login.html**

    ```
    <!-- templates/registration/login.html -->
    {% extends 'base.html' %}

    {% block title %}Log In{% endblock %}

    {% block content %}
    <h2>Log In</h2>
    <form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Log In</button>
    </form>
    {% endblock %}
    ```

    **signup.html**

    ```
    <!-- templates/registration/signup.html -->
    {% extends 'base.html' %}

    {% block title %}Sign Up{% endblock %}

    {% block content %}
    <h2>Sign Up</h2>
    <form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Sign Up</button>
    </form>
    {% endblock %}

    ```

- In <project>.urls, fill the following :

    ```
    from django.contrib import admin
    from django.urls import path , include
    from django.views.generic.base import TemplateView

    urlpatterns = [
        path('', TemplateView.as_view(template_name='home.html'), name='home'),
        path('admin/', admin.site.urls),
        path('<app>/', include('<app>.urls')),
        path('<app>/', include('django.contrib.auth.urls')),
    ]

    ```
- **Create** <app>.urls.py then **fill** it with :

    ```
    from django.urls import path
    from .views import SignUpView

    urlpatterns = [
        path('signup/', SignUpView.as_view(), name='signup'),
    ]
    ```

- Last step, In <app>.views fill with :

    ```
    from django.urls import reverse_lazy
    from django.views.generic.edit import CreateView

    from .forms import CustomUserCreationForm

    class SignUpView(CreateView):
        form_class = CustomUserCreationForm
        success_url = reverse_lazy('login')
        template_name = 'registration/signup.html'
    ``
  
  ## Part D
  - install doker :`https://www.docker.com/get-started`
- `mkdir drf-api`
- `cd drf-api`
- `poetry init -n`
- `poetry add django djangorestframework`
- `poetry shell`
- `django-admin startproject movies_project .`
- `python manage.py  migrate`
- `python manage.py  startapp movie`
- `python manage.py  createsuperuser`
- `python manage.py  runserver`
______________________________________________
- in the `settings.py` ---> INSTALLED_APPS --->add the app to project applications `'movie.apps.MovieConfig',`
- go to `movie/models.py` ----> 
```python
from django.contrib.auth import get_user_model
from django.db import models

class Movies(models.Model):
    author = models.ForeignKey(get_user_model(), on_delete=models.CASCADE)
    title = models.CharField(max_length=64)
    body = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title

```
- `python manage.py makemigrations movie`
- `python manage.py migrate`

- go to  movie/`admin.py` ----> add :
```python
from django.contrib import admin

from .models import Movies
# Register your models here.

admin.site.register(Movies)

```
- `python manage.py runserver`
- do tests
- in the `settings.py` ---> INSTALLED_APPS --->add the app to project applications `'rest_framework',`
- in the `settings.py` add:
```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASS': [
        'rest_framework.permissions.AllowAny',
    ]
}
```
- in movies_project/`urls.py`:
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/movies/', include('movie.urls')),
]

```

- create movie/`serializer.py` to convert data to json:
```python
from rest_framework import serializers

from .models import Movies

class MoviesSerializer(serializers.ModelSerializer):
    class Meta:
        fields = ('id', 'title', 'author', 'body', 'created_at')
        model = Movies

```
- in movie/`views.py`:
```python 
from django.shortcuts import render
from rest_framework import generics

from .models import Movies
from .serializer import MoviesSerializer

class MoviesList(generics.ListCreateAPIView):
    queryset = Movies.objects.all()
    serializer_class = MoviesSerializer

class MovieDetails(generics.RetrieveUpdateDestroyAPIView):
    queryset = Movies.objects.all()
    serializer_class = MoviesSerializer
```
- in movie/`urls.py`:
```python
from django.urls import path

from .views import MoviesList, MovieDetails

urlpatterns = [
    path('', MoviesList.as_view(), name='movies'),
    path('<int:pk>/', MovieDetails.as_view(), name='movie_details') 
]

```
- `python manage.py  runserver`
- in root create `Dockerfile` inside it write:
```
FROM python:3
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/
```
- in root create `docker-compose.yml` inside it write:
```
version: '3'

services:
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"

```
- in the `settings.py` --->add `ALLOWED_HOSTS = ['0.0.0.0',]`
- `python manage.py runserver 0.0.0.0:8000`
- `poetry export -f requirements.txt -o requirements.txt`
- open docker
- `docker-compose up`
- if it didnt work:
- open docker-->dashboard --->start--->open in window  settings#ALLOWED_HOSTS = ['0.0.0.0','localhost','127.0.0.1']
- or try:
- `docker-compose down`
- `docker-compose build`
- `docker-compose up`
