# rest framework之匿名用户配置

## settings.py

```python
REST_FRAMEWORK={
    # 匿名用户
    # "UNAUTHENTICATED_USER" : lambda :"匿名用户"
    "UNAUTHENTICATED_USER":None, #匿名,request.user=none
    "UNAUTHENTICATED_TOKEN":None, #匿名,request.auth=none
}
```

## 局部配置

```python
# 
authentication_classes = []
```



