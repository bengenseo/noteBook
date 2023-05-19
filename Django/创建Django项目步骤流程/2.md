# 配置settings

## 同级settings.py,创建local_settings.py

- ### settings.py

  - ### 注释

    ```python
    # DATABASES = {
    #     'default': {
    #         'ENGINE': 'django.db.backends.sqlite3',
    #         'NAME': BASE_DIR / 'db.sqlite3',
    #     }
    # }
    ```

  - ### 最后一行添加

  ```py
  try:
      from .local_settings import *
  except ImportError:
      pass
  ```

- ### 项目的__ int __.py

  ```py
  # 固定写法
  import pymysql
  pymysql.install_as_MySQLdb()  # 告诉django用pymysql代替mysqldb连接数据库
  ```

  - ### 注释

    ```py
    D:\GitHub\Django\Lamps\Lib\site-packages\django\db\backends\mysql\base.py
    # 注释掉第(35/36)行
    
    if version < (1, 4, 0):
        raise ImproperlyConfigured('mysqlclient 1.4.0 or newer is required; you have %s.' % Database.__version__)
    ```

- #### 创建local_settings.py

  - ### 配置数据库

    ```py
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.mysql',  # 数据库类型
            'NAME': 'demo',  # 数据库名字
            'HOST': 'localhost', # ip
            'PORT': '3306', 
            'USER': 'root',  # 数据库账户
            'PASSWORD': '',  # 数据库密码
    
        }
    }
    serverTimezon:Asia/Shanghai
    # 语言,时区
    LANGUAGE_CODE = 'zh-hans'
    
    TIME_ZONE = 'Asia/Shanghai'
    #静态文件(css/js/图片)
    # 静态文件保存目录的别名
    STATIC_URL = '/static/'
    # STATIC_ROOT = os.path.join(BASE_DIR, 'static')
    
    # 所有静态文件(css/js/图片)都放在我下面你配置的文件夹中
    STATICFILES_DIRS = [
        os.path.join(BASE_DIR, "static"),
    ]
    STATIC_ROOT = os.path.join(BASE_DIR, '/static/')
    # 上传文件
    MEDIA_URL = '/uploads/'
    MEDIA_ROOT = os.path.join(BASE_DIR, 'uploads')
    CKEDITOR_UPLOAD_PATH = 'allimg/'
    ```
    
    