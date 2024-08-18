
# Django Notes

all the errors which occur commonly

- when passing form see if the name you typed is correct i.e. {{form}}

- also see from where you have extended the class of form ,most of the time it is from 
    
      1.forms.ModelForm
      2.UserCreationForm


    
    ```
    from django import forms
    from .models import Tweet
    from django.contrib.auth.forms import UserCreationForm
    from django.contrib.auth.models import User

   class UserRegistrationForm(UserCreationForm):
        email=forms.EmailField()
        class Meta:
            model=User
            fields=('username','email','password1','password2')

- Above is the example of how to make a form for taking details of user


- below is one more example also shows how to make form from a model
    ```
    class TweetForm(forms.ModelForm):
        class Meta:
            model=Tweet
            fields=['text','image']

- Here is an example how to write a model 

        1.one to many
        2.many to many
        3.one to one
    while writing two and three you can use ```models.OnetoOneField(name_of_the_model,on_DELETE=cascade,related_name)```
    and similarly ```models.ManytoManyField()```.

    ```
    from django.db import models
    from django.contrib.auth.models import User
    # Create your models here.

    #one to many
    #Foriegn key is used to describe one to many relation that means one user can have many Tweet
    class Tweet(models.Model):
        user=models.ForeignKey(User,on_delete=models.CASCADE)
        image=models.ImageField(upload_to='photos/')
        time_created=models.DateTimeField(auto_now_add=True)
        updated_at=models.DateTimeField(auto_now=True)
        text=models.TextField(max_length=200)

        def __str__(self):
            return f'tweeted by {self.user}'
- When the user is deleted all the tweets of the user will be deleted this is known as models.CASCADE 
- If you want to update date automatically at every time you can use ```updated_at=DateTimeField(auto_now=true)``` or if you only want to change the date at the time of creation of you can use ```created_at=DateTimeField(auto_now_add=true)```


## QuerySet 
- How to query from Model
[Get Method](https://www.w3schools.com/django/django_queryset_get.php)
[Filter Method](https://www.w3schools.com/django/django_queryset_filter.php)
[Order By Method](https://www.w3schools.com/django/django_queryset_orderby.php)

- get gives you one object while filter returns rows that matches the description order by orders the  data

-How to send the user to the page from where he came and make a generic ```GO BACK```link
    ```
    <a href="{{request.META.HTTP_REFERER}}">GO BACK</a>
    ```

### Query Parameter
    
    def my_view(request):
        name = request.GET.get('name', 'Guest')
        return HttpResponse("Hello, " + name)
    
    In this example, if the URL is http://example.com/?name=Alice, the view will respond with "Hello, Alice". If the name parameter is not provided, it will respond with "Hello, Guest

In Django, request.GET is a dictionary-like object that provides access to the query parameters sent in a GET request.
It specifically uses the QueryDict class, which offers several methods to access and manipulate the data:
- get(key, default=None): Retrieves the value associated with the specified key. If the key is not found, it returns the default value (which is None by default).
- keys(): Returns a list of all keys in the query parameter dictionary.
- values(): Returns a list of all values in the query parameter dictionary.
- items(): Returns a list of key-value pairs (tuples) in the query parameter dictionary.
- copy(): Returns a copy of the QueryDict.
- urlencode(): Returns a URL-encoded string representation of the query parameters.

## Search Parameter
  ```
    from django.shortcuts import render,redirect
    from .models import Topic,Message,Room
    from .forms import RoomForm
    from django.db.models import Q
    from django.contrib import messages
    from django.contrib.auth import login,authenticate,logout
    # Create your views here.
 
    def home(request):
        q=request.GET.get('q') if request.GET.get('q')!=None else ""
        topics=Topic.objects.all()
        rooms=Room.objects.filter(Q(topic__name__icontains=q)|Q(name__icontains=q)|Q(host__username__icontains=q))
        return render(request,'base/index.html',{'rooms':rooms,'topics':topics})
  ```
Search bar html,See how name q is given in input and action GET is used to create the form
```
    <form class="d-flex" role="search" method="GET" action="{% url "home" %}">
              <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search" name="q">
              <button class="btn btn-outline-success" type="submit">Search</button>
            </form>
```
## How to check the user is authenticated and display stuff like this
```
    {% if request.user.is_authenticated %}
    <a href="{% url "logout" %}">logout</a>
    {% else %}
    <a href="{% url "login" %}">login</a>
    {% endif %}

```
## What  if we want to send 2 
```
     path('delete/<str:pk>/<str:room_pk>/',views.deleteMessage,name='delete'),


    {% if message.user == request.user %}
         <a href="{% url "delete" pk=message.id room_pk=room.id%}">Delete Message</a>
    {% endif %}
```

# DJANGO -REST FRAMEWORK 
