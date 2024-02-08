# rest framework框架之解析器

## 前戏:

- ### Django

  - request.POST

  - request.body

    1. 请求头要求:

       Content-Type: application/x-www-form-urlencoded

       PS: 如果请求头中的Content-Type: application/x-www-form-urlencoded,request.POST中才有值(去request.body中解析数据)

    2. 数据格式要求:

       name=bengen&age=20&gender=男

- ### 如: 

  1. form表单提交

```html
<form method....>
	<input>
</form>
```

  2. ajax提交

```js
$.ajax({
    url:"...",
    type:POST,
    data:{name:bengen,age:20}# 内部转化 name=bengen&age=20&gender=男
})
```

### 情况一:

```js
$.ajax({
    url:"...",
    type:POST,
    headers:{"Content-Type":"application/json"}
    data:{name:bengen,age:20}# 内部转化 name=bengen&age=20&gender=男
})
# body有值;POST无值
```

### 情况二:

```js
$.ajax({
    url:"...",
    type:POST,
    headers:{"Content-Type":"application/json"}
    data:JSON.STRINGFY({name:bengen,age:20})# {"name":bengen,"age":20}
})
# body有值;POST无值
```

```python
# 开始是字节格式,先转化为字符串,再转化为json格式
json.loads(str(request.body))
```

- ### rest framework

### 全局配置

```python
REST_FRAMEWORK = {
    # 解析器: 解析JSON和form表单
    "DEFAULT_PARSER_CLASSES":["rest_framework.parsers.JSONParser","rest_framework.parsers.FormParser"]
}
```

### 局部配置

```python
class ParserView(APIView):
    # parser_classes = [JSONParser,FormParser]
    """
        - 提交数据,解析器
        JSONParser: 只解析"Content-Type":"application/json"的头
        FormParser: 只解析"Content-Type":"application/x-www-form-urlencoded"的头
    """
    def post(self,request, *args,**workers):
        """
        - JSONParser
            允许用户发送JSON格式数据
            - "Content-Type":"application/json"
            - {"name":bengen,"age":20}

            1.获取用户请求
            2.获取用户请求体
            3.根据用户请求头和parser_classes = [JSONParser,FormParser]进行比较
            4.返回其中一个(如:JSONParser)对象去处理请求体
            5.得到request.data数据
        """
        print(request.data)
        '''
        # 上传
        from rest_framework.parsers import FileUploadParser
        parser_classes = [FileUploadParser]
        # 获取上传文件
        print(request.FILES)
        '''
        return HttpResponse('ParserView')
```

## 源码流程&本质

- ### 本质

  - #### 请求头:

  - #### 状态码:

  - #### 请求方法:

- ### 源码流程

  - #### dispatch:request封装

  - #### request.data

