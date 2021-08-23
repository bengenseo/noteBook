# rest framework之权限使用及源码流程

## 使用

- 继承: BasePermission

- 实现: has_permission方法

```python
from rest_framework.permissions import BasePermission
class SvipPermission(BasePermission):
    message= '必须是SVIP才能访问'
    def has_permission(self,request,view):
        if request.user.user_type != 3:
            return False
        return True

class VipPermission(BasePermission):
    message = 'VIP以上才能访问'
    def has_permission(self,request,view):
        if request.user.user_type == 1:
            return False
        return True
```

## 局部

```python
class UserInfoView(APIView):
    # 权限
    permission_classes = [VipPermission,]
    def get(self,request,*args,**kwargs):
        print(request.user)
        return HttpResponse('访问成功')
```

## 全局settings.py

```python
REST_FRAMEWORK={
    # 权限
    "DEFAULT_PERMISSION_CLASSES":['api.utils.permission.VipPermission']
}
```

## 返回值

- True
  - 有权访问
- False
  - 无权访问