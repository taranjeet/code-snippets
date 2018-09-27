## Apis

* [Using APIView](#using-apiview)
* [Using generic view](#using-generic-view)
* [Using generic view and attaching user as well](#using-generic-view-and-attaching-user-as-well)

#### Using APIView

```
# urls.py

from django.urls import path
from .views import TodoList, TodoDetail

urlpatterns = [

    path('todo', TodoList.as_view()),
    path('todo/<int:pk>/', TodoDetail.as_view()),

]
```

```
# views.py
from django.http import Http404

from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

from .models import Todo
from .serializers import TodoSerializer


class TodoList(APIView):

    def get(self, request, format=None):
        todos = Todo.objects.all()
        serializer = TodoSerializer(todos, many=True)
        return Response(serializer.data)

    def post(self, request, format=None):
        serializer = TodoSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)


class TodoDetail(APIView):

    def get_object(self, pk):
        try:
            return Todo.objects.get(pk=pk)
        except Todo.DoesNotExist:
            raise Http404

    def get(self, request, pk, format=None):
        todo = self.get_object(pk)
        todo = TodoSerializer(todo)
        return Response(todo.data)

    def put(self, request, pk, format=None):
        todo = self.get_object(pk)
        serializer = TodoSerializer(todo, data=request.DATA)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def delete(self, request, pk, format=None):
        todo = self.get_object(pk)
        todo.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```


#### Using generic view

```
# NOTE: urls remain the same as mentioned in Using APIView section.

# views.py
from rest_framework import generics
from rest_framework.reverse import reverse

from .models import Todo
from .serializers import TodoSerializer


class TodoList(generics.ListCreateAPIView):
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer


class TodoDetail(generics.RetrieveUpdateDestroyAPIView):
    serializer_class = TodoSerializer
    queryset = Todo.objects.all()
```

#### Using generic view and attaching user as well

```
# NOTE: urls remain the same as mentioned in Using APIView section.

# views.py
from rest_framework import generics
from rest_framework.reverse import reverse

from .models import Todo
from .serializers import TodoSerializer

class TodoList(generics.ListCreateAPIView):
    queryset = Todo.objects.all()
    serializer_class = TodoSerializer

    def perform_create(self, serializer):
        serializer.save(user=self.request.user)


class TodoDetail(generics.RetrieveUpdateDestroyAPIView):
    serializer_class = TodoSerializer

    def get_queryset(self):
        return Todo.objects.all().filter(user=self.request.user)
```
