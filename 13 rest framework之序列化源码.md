# rest framework之序列化源码

## 源码流程

- ### 从自定义类找构造函数

```python
# 在执行init
def __init__(self, instance=None, data=empty, **kwargs):
    pass
# 先执行new
def __new__(cls, *args, **kwargs):
    pass
```

- ### 如果找不到就往上找,如: 从继承ModelSerializer类找
  - #### 对象, Serializer类进行处理 self.to_representation

  - #### Queryset, ListSerializer类进行处理 self.to_representation

- ### 入口ser.data

  - #### self.to_representation 往上找不到,往右边找

    - #### 如果返回是报错,则换一个位置找

- ### 入口serializers.CharField

  - #### 找get_attribute

## 源码具体步骤流程

1. 实例化, 一般是将数据封装到对象: 

```python
# 再执行init
def __init__(self):
    pass
# 先执行new,返回值就是传进来的那个对象
def __new__(self):
     '''
     1.根据类创建对象,并返回
     2.执行返回值的__init__
     '''
    pass
```



1. 有多个类,就一直往上走
2. 流程

```python
class UserInfoView(APIView):
    def get(self,request,*args,**kwargs):
        users = models.UserInfo.objects.all()
        # 实例化,一般是将数据封装到对象: __new__;__init__
        '''
        many=True,执行ListSerializer对象的构造方法
        many=False,执行自定义对象(如:UserInfoSerializer)的构造方法
        '''
        obj = UserInfoSerializer(instance=users,many=True,context={'request': request})
        # 2. 调用对象的data属性
        # ListSerializer
        ret = json.dumps(obj.data,ensure_ascii=False)
        return HttpResponse(ret)
```



