# django视图之面试题和csrf补充

## 在settings中,开启csrf,则全局需要认证

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',#开启则全局需认证,注释则全局免认证
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

## 面试题

### Django五个中间件

```python
process_request
process_view
process_response
process_exception
process_render_template
```

#### 执行中间件流程

```python
用户
==>Django(process_request)
==>路由
==>返回Django再执行(process_view)
==>视图函数
==>(process_response)返回给用户
==>如果报错就执行(process_exception)返回中
	如果有render方法执行(process_render_template)
```

## 使用中间件做过什么?

### 	权限

### 	用户登陆验证

### 	Django的csrf是如何实现?

		- process_view方法
		- 先判断视图中有无装饰器(@csrf_exempt, @csrf_protect)
	
		- 去请求体或cookie中获取token



## 免认证

```python
from django.views.decorators.csrf import csrf_exempt

@csrf_exempt
def XxxView(APIView):
    pass
```

#### 方法一

```python
from django.views.decorators.csrf import csrf_exempt
from django.utils.decorators import method_decorator
class StudentsView(view):
    @method_decorator(csrf_exempt)
    def dispatch(self, request, *args, **kwargs):
        return super(StudentsView,self).dispatch( request, *args, **kwargs)
    
    def post(self, request, *args, **kwargs):
        pass
```

#### 方法二

```python
from django.views.decorators.csrf import csrf_exempt
from django.utils.decorators import method_decorator

@method_decorator(csrf_exempt,name="dispatch")
class StudentsView(view):
    
    def post(self, request, *args, **kwargs):
        pass
```





## 需认证

```python
from django.views.decorators.csrf import csrf_protect

@csrf_protect
def XxxView(APIView):
    pass
```

#### 方法一

```python
from django.views.decorators.csrf import csrf_protect
from django.utils.decorators import method_decorator
class StudentsView(view):
    @method_decorator(csrf_protect)
    def dispatch(self, request, *args, **kwargs):
        return super(StudentsView,self).dispatch( request, *args, **kwargs)
    
    def post(self, request, *args, **kwargs):
        pass
```

#### 方法二

```python
from django.views.decorators.csrf import csrf_protect
from django.utils.decorators import method_decorator

@method_decorator(csrf_protect,name="dispatch")
class StudentsView(view):
    
    def post(self, request, *args, **kwargs):
        pass
```

## 取消csrf认证

- 装饰器要加到dispatch方法上@method_decorator装饰

## rest framework 认证流程(封装request)



## rest framework 权限流程



## rest framework 节流流程



## 代码:

- #### 认证demo

- #### 权限demo(用户类型不同,权限不同)

- #### 节流demo(匿名,登陆[vip,svip])

- #### 三组件组合

## 分页总结

- ### 数据量大的话,如何做分页?

  - #### 数据库性能相关?
