

## 路由: 方法一

```python
# http://127.0.0.1:99/api/v1/view1/?format=json
url(r'^(?P<version>[v1|v2]+)/(?i)view1/$', views.view1View.as_view({'get':'list'})),
# http://127.0.0.1:99/api/v1/view1.json
url(r'^(?P<version>[v1|v2]+)/(?i)view1\.(?P<format>\w+)$', views.view1View.as_view({'get':'list'})),
url(r'^(?P<version>[v1|v2]+)/(?i)view1/(?P<pk>\d+)/$', views.view1View.as_view({'get':'retrieve','delete':'destroy','put':'update','path':'partial_update'})),
# http://127.0.0.1:99/api/v1/view1/1.json
url(r'^(?P<version>[v1|v2]+)/(?i)view1/(?P<pk>\d+)\.(?P<format>\w+)$', views.view1View.as_view({'get':'retrieve','delete':'destroy','put':'update','path':'partial_update'})),
```

## 路由: 方法二(只有继承了增删改查,才能用)

```python
from rest_framework import routers
router = routers.DefaultRouter()
router.register(r's',views.view1View)
# 含翻页
router.register(r'rt',views.view1View)
urlpatterns=[
    url(r'^(?P<version>[v1|v2]+)/',include(router.urls))
]
```

## 渲染器

- ### 局部设置

```python
from rest_framework.renderers import JSONRenderer,BrowsableAPIRenderer
renders_calsses=[JSONRenderer,BrowsableAPIRenderer]
```

- ### 全局设置

```python
REST_FRAMEWORK = {
    # 渲染器
    "DEFAULT_RENDERER_CLASSES":[
        'rest_framework.renderers.JSONRenderer',
        'rest_framework.renderers.BrowsableAPIRenderer'
    ]
}
```

