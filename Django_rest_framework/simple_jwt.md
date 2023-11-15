# Simple JWT

***

### Simple JWT without password

- Simple JWT without password , what that mean ?
- first lets say why without password :

1. in some project there is no need to password , means user for login should not send the password,
   why to do that ? I don't know it's on you, that what your project need , maybe your project is working
   with one time password like OTP(SMS) or .... that remove need of password for users .

2. I have searched alot and only way to make simple_jwt work without password is making a backend
   from BaseBackend of django to work , Because simple_jwt give access token only if get a username + password .

3. So i have look the simple_jwt code and searched and find a good solution

*** 

1. First we install simple_jwt , and set needed settings for django
2. Ok now lets import some things that we need from simple_jwt

```python
from rest_framework_simplejwt.tokens import RefreshToken, AccessToken, BlacklistedToken
```

we use some classes that are in simple_jwt so we dont do the basics

3. We can use AccessToken to create a access token and RefreshToken for refresh, these classes have a method
   called for_user() we put a User object in it to make a access token for us to send to client 
```python
from django.contrib.auth.models import User
from rest_framework.views import APIView
from rest_framework.response import Response


class LoginView(APIView):
    # choice a user to build a token for him
    user = User.objects.get(username='Example')

    # access token for that user 
    access_token = AccessToken.for_user(user=user)
    # refresh token for that user 
    refresh_token = RefreshToken.for_user(user=user)
    
    return Response({'access': str(access_token),
                     'refresh': str(refresh_token)})

```
As you can see be have a access token and a refresh token and we can now send these to our front-end
you can use the method top for your login view after all other views of simple jwt is working fine.

4. If we just use the simple_jwt TokenBlacklistView or TokenRefreshView with our manually built tokens 
it will work fine without any problem 
```python
from rest_framework_simplejwt.views import TokenBlacklistView, TokenRefreshView
from rest_framework import permissions


# It will put Refresh Token in blacklist
class LogoutView(TokenBlacklistView):
    permission_classes = [permissions.IsAuthenticated]


# It get Refresh token and give new access and refresh token 
class RefreshView(TokenRefreshView):
    permission_classes = [permissions.IsAuthenticated]
```
NOTE: remember TokenBlackListView put refresh token in black list making getting new token impossible with refresh token
but front-end must delete access token to fully logout 

***
# Conclusion 

- With explanation above we can handle the authentication in django with simple_jwt without changing backend 
- If using this method remember main authentication must be done by your self in Login view , i teach you how
to create tokens 

THANKS ! and please leave a star ;)