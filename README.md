# JWT Tutorial with Django Rest Framework: A Quick Example

## Introduction
A quick and simple example to implementing JWT to secure your backend Django API

## Installation
#### Rest Framework SimpleJWT
`$ pip install djangorestframework-simplejwt`

## Edit settings.py
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

## Edit urls.py
    from rest_framework_simplejwt.views import (
        TokenObtainPairView,
        TokenRefreshView,
    )

    urlpatterns = [
        ...
        path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
        path('api/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
        ...
    ]

## Edit views.py to protect your desired backend API
    from rest_framework.permissions import IsAuthenticated

    class HelloView(APIView):
        permission_classes = (IsAuthenticated,) ## Add this "permission_classes" to enable JWT
        def get(self, request):
            content = {'message': 'Hello, World!'}
            return Response(content)
