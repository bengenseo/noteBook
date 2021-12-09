# rest framework之视图

## 无用

```python
from rest_framework.generics import GenericAPIView
from api.utils.serializers.pager import PageSerializer
from rest_framework.response import Response
class view1View(GenericAPIView):
    queryset = models.Role.objects.all()
    serializer_class = PageSerializer
    pagination_class = PageNumberPagination
    def get(self,request, *args,**kwargs):
        # 获取数据
        roles = self.get_queryset().order_by('id') # models.Role.objects.all()
        # [1,1000,] 获取 [1,10]
        pager_roles = self.paginate_queryset(roles)
        # 序列化
        ser = self.get_serializer(instance=pager_roles,many=True)
        return Response(ser.data)
```

## GenericViewSet

- ### 路由(urls.py)

```python
url(r'^(?P<version>[v1|v2]+)/(?i)view1/$', views.view1View.as_view({'get':'list'})),
```

- ### 视图

```python
from rest_framework.viewsets import GenericViewSet
from api.utils.serializers.pager import PageSerializer
from rest_framework.response import Response
class view1View(GenericViewSet):
    queryset = models.Role.objects.all()
    serializer_class = PageSerializer
    pagination_class = PageNumberPagination
    def list(self,request, *args,**kwargs):
        # 获取数据
        roles = self.get_queryset().order_by('id') # models.Role.objects.all()
        # [1,1000,] 获取 [1,10]
        pager_roles = self.paginate_queryset(roles)
        # 序列化
        ser = self.get_serializer(instance=pager_roles,many=True)
        return Response(ser.data)
```

## ModelViewSet

- ### 继承

```python
mixins.CreateModelMixin, # 增
mixins.RetrieveModelMixin, # 获取单条数据,传ID
mixins.UpdateModelMixin, # 更新,传ID
mixins.DestroyModelMixin, # 删除
mixins.ListModelMixin, # 获取所有数据
GenericViewSet
```

- ### 路由

```python
url(r'^(?P<version>[v1|v2]+)/(?i)view1/$', views.view1View.as_view({'get':'list'})),
url(r'^(?P<version>[v1|v2]+)/(?i)view1/(?P<pk>\d+)/$', views.view1View.as_view({'get':'retrieve','delete':'destroy','put':'update','path':'partial_update'})),
```

- ### 视图

  - 归类FilerBackend.py

```python
from rest_framework.filters import BaseFilterBackend
class PageFilerBackend(BaseFilterBackend):
    def filter_queryset(self, request, queryset, view):
        val=request.query_params.get('archives')
        return queryset.filter(archives_id=val)
```

```python
from rest_framework.viewsets import GenericViewSet,ModelViewSet
from rest_framework.mixins import CreateModelMixin,DestroyModelMixin,UpdateModelMixin,ListModelMixin,RetrieveModelMixin
from api.utils.serializers.pager import PageSerializer
from rest_framework.response import Response
class view1View(ModelViewSet):
    queryset = models.Role.objects.all()
    filter_backends = [PageFilerBackend,]
    serializer_class = PageSerializer
    pagination_class = PageNumberPagination
    
    # def put(self,request,pk):
    #     return self.update(request,pk)
    # def delete(self,request,pk):
    #     return self.destroy(request,pk)
    # def post(self,request,pk):
    #     return self.create(request,pk)
    # def get(self,request,pk):
    #     return self.list(request,pk)
    # def path(self,request,pk):
    #     return self.partial_update(request,pk)
```

## 总结

- ### 查找,增加(不需要传id)

  - #### 不需要传id

    - ```python
      from rest_framework.generics import CreateView,ListView
      class NewsView(CreateView,ListView):
          queryset = models.Role.objects.all()
          serializer_class = PageSerializer
          # 钩子方法
          def get_serializer_class(self):
              if self.request.method=='GET':
                  return PageSerializer
              elif  self.request.method=='POST':
                  return Page2Serializer
      ```

    - ```python
      from rest_framework.generics import ListCreateAPIView
      class NewsView(ListCreateAPIView):
          queryset = models.Role.objects.all()
          serializer_class = PageSerializer
          # 钩子方法
          def get_serializer_class(self):
              if self.request.method=='GET':
                  return PageSerializer
              elif  self.request.method=='POST':
                  return Page2Serializer
      ```

    - 

  - #### 需要传id

    - ```python
      from rest_framework.generics import UpdateView,RetrieveView,DestroyAPIView
      class NewsView(UpdateView,RetrieveView,DestroyAPIView):
          queryset = models.Role.objects.all()
          serializer_class = PageSerializer
      ```

    - ```python
      from rest_framework.generics import RetrieveUpdateDestroyAPIView
      class NewsView(RetrieveUpdateDestroyAPIView):
          queryset = models.Role.objects.all()
          serializer_class = PageSerializer
      ```

    - ---

- 基本增删改查等
  - ModelViewSet

- 增删
  - CreateModelMixin
  - DestroyModelMixin
- 复杂逻辑
  - GenericViewSet 或 APIView