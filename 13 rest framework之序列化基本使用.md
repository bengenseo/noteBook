# rest framework框架之序列化基本使用

## 自定义

```python
from rest_framework import serializers
class RoleSerializer(serializers.Serializer):
    id = serializers.IntegerField()
    title = serializers.CharField()
class RolesView(APIView):
    def get(self,request,*args,**kwargs):
        # 方法一:
        # roles=models.Role.objects.all().values("id","title")
        # roles=list(roles)
        # # ensure_ascii=False 中文编码不做转义
        # ret = json.dumps(roles,ensure_ascii=False)

        # 方法二: 对于[obj,obj,obj]格式化
        # roles = models.Role.objects.all()
        # ser = RoleSerializer(instance=roles,many=True)
        # # ser.data 序列化,有序字典
        # ret = json.dumps(ser.data,ensure_ascii=False)

        # 一个对象,many=False,否则报错
        role = models.Role.objects.all().first()
        ser = RoleSerializer(instance=role, many=False)
        # ser.data 序列化,有序字典,已经是转化完成的结果
        ret = json.dumps(ser.data, ensure_ascii=False)
        return HttpResponse(ret)
```



## 继承: Serializer

- ### 序列化

```python
from rest_framework import serializers
class UserInfoSerializer(serializers.Serializer):
    type = serializers.CharField(source="user_type") # row.user_type
    rank = serializers.CharField(source="get_user_type_display")  # row.get_user_type_display()
    username = serializers.CharField()
    password = serializers.CharField()
    gp = serializers.CharField(source='groups.title') # 如果是groups,它是对象. 指定字段: groups.title

    # 数据库ManyToManyField,需要SerializerMethodField()才能完成序列化
    role = serializers.SerializerMethodField()# 自定义显示
    # 自定义方法
    def get_role(self,row):
        role_obj_list = row.Role.all()

        ret = []
        for item in role_obj_list:
            ret.append({"id":item.id,"title":item.title})
        return ret
```

- ### 视图

```python
class UserInfoView(APIView):
    def get(self,request,*args,**kwargs):
        users = models.UserInfo.objects.all()
        obj = UserInfoSerializer(instance=users,many=True)
        ret = json.dumps(obj.data,ensure_ascii=False)
        return HttpResponse(ret)
```

## 继承: ModelSerializer

- ### 序列化

```python
from rest_framework import serializers
class UserInfoSerializer(serializers.ModelSerializer):
    rank = serializers.CharField(source="get_user_type_display")  # row.get_user_type_display()
    role = serializers.SerializerMethodField()  # 自定义显示
    class Meta:
        model = models.UserInfo
        # fields= "__all__"
        fields = ["id","username","password","rank","role"]

    # 自定义方法
    def get_role(self, row):
        role_obj_list = row.Role.all()

        ret = []
        for item in role_obj_list:
            ret.append({"id": item.id, "title": item.title})
        return ret
```

- 视图

```python
class UserInfoView(APIView):
    def get(self,request,*args,**kwargs):
        users = models.UserInfo.objects.all()
        obj = UserInfoSerializer(instance=users,many=True)
        ret = json.dumps(obj.data,ensure_ascii=False)
        return HttpResponse(ret)
```

## 自动序列化连表(depth 控制深度)

```python
from rest_framework import serializers
class UserInfoSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.UserInfo
        fields= "__all__"
        # fields = ["id","username","password","rank","role"]
        depth = 1 # 控制层级关联深度 0 ~ 10

class UserInfoView(APIView):
    def get(self,request,*args,**kwargs):
        users = models.UserInfo.objects.all()
        obj = UserInfoSerializer(instance=users,many=True)
        ret = json.dumps(obj.data,ensure_ascii=False)
        return HttpResponse(ret)
```

## HyperlinkedIdentityField反向生成URL

```python
class UserInfoSerializer(serializers.ModelSerializer):
    # 2. 反向生成链接 HyperlinkedIdentityField
    # lookup_field="groups_id",要指定groups数据表的ID,否则生成链接会出错
    # lookup_url_kwarg="pk",pk是urls.py里的正则字段
    groups = serializers.HyperlinkedIdentityField(view_name="gp",lookup_field="groups_id",lookup_url_kwarg="pk")
    class Meta:
        model = models.UserInfo
        # fields= "__all__"
        fields = ["id", "username", "password", "groups", "Role"]

class UserInfoView(APIView):
    def get(self,request,*args,**kwargs):
        users = models.UserInfo.objects.all()
        # 3. 用 HyperlinkedIdentityField 需要添加 context={'request': request}
        obj = UserInfoSerializer(instance=users,many=True,context={'request': request})
        ret = json.dumps(obj.data,ensure_ascii=False)
        return HttpResponse(ret)


class GroupSerializer(serializers.ModelSerializer):
    class Meta:
        model = models.UserGroup
        fields = "__all__"

class GroupView(APIView):
    def get(self, request, *args, **kwargs):
        # 1. 从urls.py获取pk
        pk = kwargs.get('pk')
        obj = models.UserGroup.objects.filter(pk=pk).first()
        ser = GroupSerializer(instance=obj,many=False)
        ret = json.dumps(ser.data,ensure_ascii=False)
        return HttpResponse(ret)
```

