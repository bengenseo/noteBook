# rest framework之序列化验证用户请求数据

```python
class Validator(object):
    #  # value要以某个指定字段开头
    def __init__(self,base):
        self.base = base
    def __call__(self,value):
        if not value.startswith(self.base):
            message = "标题以 %s 为开头" % self.base
            raise serializers.ValidationError(message)
    def set_context(self,serializer_field):
        # 执行验证之前调用,serializer_fields是当前字段对象
        pass

class UserGroupSerializer(serializers.Serializer):
    name = serializers.CharField(error_messages={"required":"标题不能为空"},validators=[Validator('开'),])

class UserGroupView(APIView):
    def post(self,request,*args,**kwargs):
        # print(request.data)
        ser=UserGroupSerializer(data=request.data)
        if ser.is_valid():
            print(ser.validated_data,ser.validated_data['name'])
        else:
            print(ser.errors)
        return HttpResponse('提交数据')
```

## 问: 自定义验证规则时,需要钩子函数?请问钩子函数如何写?

- ### is_valid-->self.run_validation-->to_internal_value-->to_internal_value-->validate_字段名称(执行字段验证,钩子方法)-->validate_method(钩子验证方法)

