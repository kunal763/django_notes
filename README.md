
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
