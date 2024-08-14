
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
    
- If you want to update date automatically at every time you can use ```updated_at=DateTimeField(auto_now=true)``` or if you only want to change the date at the time of creation of you can use ```created_at=DateTimeField(auto_now_add=true)```


## QuerySet 
- How to query from Model
[Get Method](https://www.w3schools.com/django/django_queryset_get.php)
[Filter Method](https://www.w3schools.com/django/django_queryset_filter.php)
[Order By Method](https://www.w3schools.com/django/django_queryset_orderby.php)

- get gives you one object while filter returns rows that matches the description order by orders the  data
