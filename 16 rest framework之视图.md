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

```python
from rest_framework.viewsets import GenericViewSet,ModelViewSet
from rest_framework.mixins import CreateModelMixin,DestroyModelMixin,UpdateModelMixin,ListModelMixin,RetrieveModelMixin
from api.utils.serializers.pager import PageSerializer
from rest_framework.response import Response
class view1View(ModelViewSet):
    queryset = models.Role.objects.all()
    serializer_class = PageSerializer
    pagination_class = PageNumberPagination
```

## 总结

- 基本增删改查等
  - ModelViewSet

- 增删
  - CreateModelMixin
  - DestroyModelMixin
- 复杂逻辑
  - GenericViewSet 或 APIView