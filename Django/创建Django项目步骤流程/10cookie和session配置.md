# cookie和session配置

## cookie

- ### max_age 时间,None->关闭浏览器失效过期

- ### path 栏目层级

- ### domain='.bengenseo.com'

- ### secure False->http True->https

- ### httponly False->如js请求得到值 True->值为空

```py
res.set_cookie('键','值',max_age=100,path='/',domain='域名',secure=False,httponly=False)
```

## session配置

- ### settings.py

  ```py
  SESSION_ENGINE = "django.contrib.sessions.backends.db"#数据库
  SESSION_ENGINE = 'django.contrib.sessions.backends.cached_db' #缓存和数据库
  
  # Session的cookie保存的路径（默认）
  #SESSION_COOKIE_PATH = '/'
  
  # Session的cookie保存在浏览器上时的key，即：sessionid＝随机字符串（默认）
  SESSION_COOKIE_NAME = 'sessionid'
  # Session的cookie失效日期（2周）（默认）
  SESSION_COOKIE_AGE = 60 * 60 * 24 * 7 * 2
  # Session的cookie保存的域名（默认）
  SESSION_COOKIE_DOMAIN = None
  # 是否Https传输cookie（默认）
  SESSION_COOKIE_SECURE = False
  # 是否Session的cookie只支持http传输（默认）
  SESSION_COOKIE_HTTPONLY = True
  # 是否每次请求都保存Session，默认修改之后才保存（默认）
  SESSION_SAVE_EVERY_REQUEST = False
  # 是否关闭浏览器使得Session过期（默认）
  SESSION_EXPIRE_AT_BROWSER_CLOSE = False
  # 存储session数据默认使用的模块
  SESSION_ENGINE = 'django.contrib.sessions.backends.cached_db'
  # session数据的序列化类
  SESSION_SERIALIZER = 'django.contrib.sessions.serializers.JSONSerializer'
  ```

  

| 方法                              | 描述                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| request.session['键']=值         | 以键值对的格式写session                                      |
| request.session.get('键',默认值)  | 根据键读取值                                                 |
| request.session['键']             | 同上：根据键读取值                                           |
| request.session.clear()          | 清除当前这个用户的session数据                                |
| request.session.flush()          | 删除session并删除在浏览器中存储的sessionid，一般在注销的时候用得比较多 |
| del request.session['键']         | 删除session中的指定键及值，在存储中只删除某个键及对应的值    |
| request.pop['键']                | 删除session中的指定键及值，并返回删除的键值对                |
| request.keys                      | 从session中获取所有的键                                      |
| request.items                     | 从session中获取所有的值                                      |
| request.session.set_expiry(value) | 设置session数据有效时间； 如果不设置，默认过期时间为两周     |
| request.session.clear_expiry()    | 清除过期的session，Django不会自动清理过期的session。需手动清理 |