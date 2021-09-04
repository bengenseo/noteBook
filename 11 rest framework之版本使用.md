# rest framework框架之版本使用

## URL中通过GET传参

```python
from django.shortcuts import render,HttpResponse
from rest_framework.views import APIView
from rest_framework.versioning import QueryParameterVersioning

class ParamVersion(object):
    # 获取版本
    def determine_version(self,request,*args,**kwargs):
        version = request.query_params.get('version')
        return version
class UsersView(APIView):
    # 自定义ParamVersion
    # versioning_class = ParamVersion

    # 内置QueryParameterVersioning
    versioning_class = QueryParameterVersioning
    def get(self,request,*args,**kwargs):
        # version = request._request.GET.get('version')
        # print(version)
        # version = request.query_params.get('version')
        # print(version)
        print(request.version)
        return HttpResponse('用户列表')
```

## URL中通过GET传参(推荐)

```python
from django.shortcuts import render,HttpResponse
from rest_framework.views import APIView
# from rest_framework.versioning import URLPathVersioning

class UsersView(APIView):
    # 内置推荐
    # versioning_class = URLPathVersioning
    def get(self,request,*args,**kwargs):
        # 获取版本
        print(request.version)
        # 获取处理版本的对象
        print(request.versioning_scheme)
        return HttpResponse('用户列表')
```

### 配置settings.py

```python
REST_FRAMEWORK = {
    # 处理版本的类
    "DEFAULT_VERSIONING_CLASS":'rest_framework.versioning.URLPathVersioning',
    # 默认版本
    "DEFAULT_VERSION":'v1',
    # 允许所有版本
    "ALLOWED_VERSIONS":['v1','v2'],
    # 获取的值
    "VERSION_PARAM":'version',
}
```

### 配置urls.py

```python
urlpatterns = [
    # (?i) 不区分大小写
    url(r'^(?P<version>[v1|v2]+)/(?i)users/$', views.UsersView.as_view()),
]
```

## 反向生成URL

```python
from django.shortcuts import render,HttpResponse
from rest_framework.views import APIView

class UsersView(APIView):
    def get(self,request,*args,**kwargs):
        # 反向生成URL
        # 绝对路径
        u_url = request.versioning_scheme.reverse(viewname='users_url',request=request)
        print(u_url)
        # 反向生成URL(Django)
        # 相对路径
        from django.urls import reverse
        d_url = reverse(viewname='users_url',kwargs={"version": "v1"})
        print(d_url)
        return HttpResponse('用户列表')

```

### 配置urls.py

```python
urlpatterns = [
    # (?i) 不区分大小写
    url(r'^(?P<version>[v1|v2]+)/(?i)users/$', views.UsersView.as_view(),name='users_url'),
]
```

