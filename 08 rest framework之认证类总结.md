# rest framework之认证类总结

## 使用

### 创建类

1. 继承: BaseAuthentication

2. 实现: authenticate

```python
from rest_framework.authentication import BaseAuthentication
class Authentication(BaseAuthentication):
    # 登陆认证处理
    def authenticate(self,request):
		*

    def authenticate_header(self,request):
        pass
```

### 返回值

1. None: 我不管,下一个认证来执行

2. 抛出异常:

```python
from rest_framework import exceptions
raise exceptions.AuthenticationFailed('用户认证失败')
```

3. 元祖

   (元素1,元素2)

   元素1赋值给request.user

   元素2赋值给request.auth

