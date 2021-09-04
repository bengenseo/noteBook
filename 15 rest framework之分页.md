# rest framework之分页

## 分页

## 全局配置

```python
REST_FRAMEWORK = {
    #分页
    # http://127.0.0.1:99/api/v1/page1/?page=2&size=2
    # 排序
    "DEFAULT_PAGINATION_CLASS":'rest_framework.pagination.PageNumberPagination',
    # 默认条数
    "PAGE_SIZE":2,
    # 最大条数
    "MAX_PAGE_SIZE": 5,
    # 页数
    "PAGE_SIZE_PARAM": 'page',
    # 条数
    "PAGE_SIZE_QUERY_PARAM": 'size',
}
```

## 局部配置

```python
class MyPageNumberPagination(PageNumberPagination):
    # http://127.0.0.1:99/api/v1/page1/?page=2&size=2
    # 默认条数
    page_size = 5
    # 页码
    page_query_param = 'page'
    # 条码
    page_size_query_param = 'size'
```

## 整体代码

```python
from rest_framework import serializers
# 数据库
from api import models
# 分页
from rest_framework.pagination import PageNumberPagination
class MyPageNumberPagination(PageNumberPagination):
    # http://127.0.0.1:99/api/v1/page1/?page=2&size=2
    # 默认条数
    page_size = 5
    # 页码
    page_query_param = 'page'
    # 条码
    page_size_query_param = 'size'
class PageSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.Role
        fields="__all__"
class Page1View(APIView):
    def get(self,request, *args,**kwargs):
        # 获取数据库中所有数据
        # order_by('id'),防止分页排序报错
        roles=models.Role.objects.all().order_by('id')
        # 创建分页对象
        # pg = PageNumberPagination()
        pg = MyPageNumberPagination()
        # 在数据库中获取分页的数据
        pg_roles = pg.paginate_queryset(queryset=roles,request=request,view=self)
        # 对数据进行序列化
        ser = PageSerializer(instance=pg_roles, many=True)
        # ret = json.dumps(ser.data,ensure_ascii=False)
        # return HttpResponse(ret)
        # return Response(ser.data)
        # 返回上页下页
        return pg.get_paginated_response(ser.data)
```



- ### 分页,看第N页,每页显示N条数据

```python
from rest_framework.pagination import PageNumberPagination
class MyPageNumberPagination(PageNumberPagination):
    # http://127.0.0.1:99/api/v1/page1/?page=2&size=2
    # 默认条数
    page_size = 5
    # 页码
    page_query_param = 'page'
    # 条码
    page_size_query_param = 'size'
```

- ### 分页,在N个位置,向后查看N条数据

```python
# 分页
from rest_framework.pagination import LimitOffsetPagination
class MyLimitOffsetPagination(LimitOffsetPagination):
	default_limit = 5
	max_limit = 30
	# 选N条
	limit_query_param = 'limit'
	# 从N条开始
	offset_query_param = 'offset'
```

- ### 加密分页,上一页和下一页

```python
from rest_framework.pagination import CursorPagination
class MyCursorPagination(CursorPagination):
    # 默认条数
    page_size = 5
    # 页码
    cursor_query_param = 'page'
    # 排序
    ordering = 'id'
    # 条码
    page_size_query_param = 'size'
    # 最大条数
    max_page_size= 7
```

