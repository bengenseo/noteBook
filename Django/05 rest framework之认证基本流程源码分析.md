# rest framework之认证基本流程源码分析

## 源码流程图

![](.\assets\image-20210811230854684.png)

- ### 入口 self.dispatch

## settings.py

```python
REST_FRAMEWORK={
    # 全局使用认证类
    "DEFAULT_AUTHENTICATION_CLASSES":['api.utils.auth.FirstAuthentication','api.utils.auth.Authentication'], # 可以放多个路径的类
}
```

## 局部配置

```python
#  
authentication_classes = [Authentication,]
```



