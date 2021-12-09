# restframework_jwt认证登陆

## 什么是jwt?有哪些优势?

```python
一般在前后端分离时,用于做用户认证(登陆)使用的技术.
jwt的实现原理:
    用户登陆成功之后,会给前端返回一段token.
    token是由.分割的三段组成
    	第一段: 类型和算法信息
        第二段: 用户信息+超时时间
        第三段: hs256(将前两段拼接)加密+base64url(加盐)
    以后前端在发来信息时
    	超时验证
        token合法性校验
优势:
    token只在前端保存,后端只负责校验
    内部集成超时时间,后端可以根据时间进行校验是否超时
    由于内部存在hash256加密,所以用户不可以修改token,只要 修改认证就失败
```



## settings.py

```python
INSTALLED_APPS = [
    'rest_framework_jwt',
]
# JWT认证
import datetime
JWT_AUTH={
    "JWT_EXPIRATION_DELTA":datetime.timedelta(minutes=5) # 5分钟过期
}
```

## views.py

```python
class LoginView(APIView):
    def get(self, request, *args, **kwargs):
        import jwt
        from rest_framework import exceptions
        from rest_framework_jwt.settings import api_settings
        jwt_decode_handler = api_settings.JWT_DECODE_HANDLER
        jwt_value = request.query_params.get('token')
        try:
            payload = jwt_decode_handler(jwt_value)
        except jwt.ExpiredSignature:
            msg = "签名已过期"
            raise exceptions.AuthenticationFailed(msg)
        except jwt.DecodeError:
            msg = "token格式错误"
            raise exceptions.AuthenticationFailed(msg)
        except jwt.InvalidTokenError:
            msg = "认证失败"
            raise exceptions.AuthenticationFailed(msg)
        print(payload)
        return Response("文章列表")

    def post(self, request, *args, **kwargs):
        from rest_framework_jwt.views import api_settings
        jwt_payload_handler = api_settings.JWT_PAYLOAD_HANDLER
        jwt_encode_handler = api_settings.JWT_ENCODE_HANDLER
        # user = models.UserInfo.objects.filter(username=request.data.get('username'),password=request.data.get('password')).first()
        user = models.UserInfo.objects.filter(**request.data).first()
        if not user:
            return Response({"code": "10001", "error": "用户名或密码错误"})
        payload = jwt_payload_handler(user)
        token = jwt_encode_handler(payload)
        return Response({"code": "200", "data": token})
```

## urls.py

```python
# jwt认证
from rest_framework_jwt.views import obtain_jwt_token

urlpatterns += [
    url(r'^jwt/login/$', obtain_jwt_token),# obtain_jwt_token.as_view()
]
```

