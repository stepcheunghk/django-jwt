# JWT Tutorial with Django Rest Framework: A Quick Example

## Introduction
A quick and simple example to implementing JWT to secure your backend Django API

### 1. Installation
#### Rest Framework SimpleJWT
`$ pip install djangorestframework-simplejwt`

### 2. Edit settings.py
    INSTALLED_APPS = [
        ...
        'rest_framework_simplejwt',
        ...
    ]

    ...

    REST_FRAMEWORK = {
        'DEFAULT_AUTHENTICATION_CLASSES': [
            ...
            'rest_framework_simplejwt.authentication.JWTAuthentication',
        ],
    }

### 3. Edit urls.py
    from rest_framework_simplejwt.views import (
        TokenObtainPairView,
    )

    urlpatterns = [
        ...
        path('hello/', views.HelloView.as_view(), name='hello'), ## Sample API for JWT Example
        path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
        ...
    ]

There are two URLs in this example.

'hello/': sample API
'api/token/': API to get the JWT token

### 4. Edit views.py to protect your backend API

Add "permission_classes = (IsAuthenticated,)" to the view you want to protect

    from rest_framework.permissions import IsAuthenticated

    class HelloView(APIView):
        permission_classes = (IsAuthenticated,) ## Add this "permission_classes" to enable JWT
        def get(self, request):
            content = {'message': 'Hello, World!'}
            return Response(content)

### 5. Verify that JWT is working properly
    curl http://YourBackendAPI/hello/

Because it did not carry the JWT token to make the API call, it should return error message. For example,

> {"detail":"Authentication credentials were not provided."}

In order to make backend API calls, we should obtain the token first.

    curl \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"username": "superuser", "password": "Cmhk1234!"}' \
    http://YourBackendAPI/api/token/

It should return two tokens (refresh and access). For example,

```{"refresh":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTYzMTIwOTkxMywianRpIjoiZDI3Nzk3YzhhYmU3NGZjMTgxY2UyYmM4YjYxODdkNTAiLCJ1c2VyX2lkIjoxMTZ9.Z3rvROe8FxHQbex_IpRY47fN4nR74K1ZefrzektFnSY","access":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjMxOTg3NTEzLCJqdGkiOiJiM2Q2NzNjNWNiNmY0NWM2YWQ5NzljM2ZlM2ZmZTRhNSIsInVzZXJfaWQiOjExNn0.IZTuRwoU36IxavkheNUH7N1NX9zxA6kloAxD4hTPCNU"}```

We use "access" token to access our API.

    curl \
    -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNjMxOTg3NTEzLCJqdGkiOiJiM2Q2NzNjNWNiNmY0NWM2YWQ5NzljM2ZlM2ZmZTRhNSIsInVzZXJfaWQiOjExNn0.IZTuRwoU36IxavkheNUH7N1NX9zxA6kloAxD4hTPCNU" \
    http://YourBackendAPI/hello/

It should return success message. For example,

> {"message":"Hello, World!"}
