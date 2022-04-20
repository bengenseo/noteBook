# 配置步骤

## 上线一定要创建

- ## 单独Nginx

  - [项目部署.pdf](images\项目部署.pdf) 

- ## 基于宝塔

- ### 同级settings.py,创建local_settings.py

  ```python
  DEBUG = False
  ALLOWED_HOSTS = ['*']
  STATIC_URL = '/static/'
  STATIC_ROOT='/data/auction_static/'
  CACHES = {
      "default": {
          "BACKEND": "django_redis.cache.RedisCache",
          "LOCATION": "redis://10.211.55.25:6379",
          "OPTIONS": {
              "CLIENT_CLASS": "django_redis.client.DefaultClient",
              "CONNECTION_POOL_KWARGS": {"max_connections": 100},
              # "PASSWORD": "woshiniba",
          }
      }
  }
  ```

- ### settings.py写入

  ```python
  try:
      from .local_settings import *
  except ImportError:
      pass
  ```

- ### 根目录创建.gitignore文件,把local_settings.py写入,就直接忽略不上传此文件

  ```python
  local_settings.py
  ```

- ### 收集静态文件

  ```python
  cd /www/wwwroot/blog
  python manage.py collectstatic
  ```
  
  - #### Nginx
  
    - 反向代理物理路径
    
    ```python
    /www/server/panel/vhost/nginx/proxy/*.conf
    ```
  
    - 宝塔-反向代理
  
      - 配置文件 - 删除
    
        ```python
        #PROXY-START/
        location ~* \.(gif|png|jpg|css|js|woff|woff2)$
        {
        	proxy_pass http://127.0.0.1:8897;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header REMOTE-HOST $remote_addr;
            expires 12h;
        }
        ```
  
      - 改成
    
        ```python
        location /static/ {
            alias /www/wwwroot/blog/static/;
        }
        location  /robots.txt {
            alias  /www/wwwroot/blog/static/robots.txt;
        }
        location  /favicon.ico {
            alias  /www/wwwroot/blog/static/favicon.ico;
        }
        ```
  
- #### 配置UWSGI.ini更改三项

  ```python
  #添加配置选择
  [uwsgi]
  #配置和nginx连接的socket连接
  socket=127.0.0.1:8997
  #配置项目路径，项目的所在目录
  chdir=/www/wwwroot/blog/
  #配置wsgi接口模块文件路径,也就是wsgi.py这个文件所在的目录
  wsgi-file=blog/wsgi.py
  #配置启动的进程数
  processes=4
  #配置每个进程的线程数
  threads=2
  #配置启动管理主进程
  master=True
  #配置存放主进程的进程号文件
  pidfile=uwsgi.pid
  #配置dump日志记录
  daemonize=uwsgi.log`
  ```

  

  

  

- ### 启动虚拟环境

  ```python
  source /www/wwwroot/blog/blog_venv/bin/activate
  ```

  

  

## 版本依赖

- 指令

```python
python -m pip freeze > requirements.txt
```



```text
Django==1.11.29
#redis服务器
django_redis==4.11.0
# rest_framework
djangorestframework==3.9.0
# 跨域问题
django-cors-headers==3.2.0
#身份验证 JSON Web Token(jwt)
pip install djangorestframework-jwt==1.11.0



腾讯云短信模块
tencentcloud-sdk-python


# 包括一些内置依赖
asgiref==3.4.1
certifi==2021.5.30
charset-normalizer==2.0.3
Django==1.11.29
django-cors-headers==3.2.0
django-redis==4.11.0
djangorestframework==3.9.0
idna==3.2
pytz==2021.1
qcloud-python-sts==3.0.5
redis==3.5.3
requests==2.26.0
sqlparse==0.4.1
tencentcloud-sdk-python==3.0.442
typing-extensions==3.10.0.0
urllib3==1.26.6
pillow==8.3.2
django-ckeditor==5.4.0
django-haystack==2.7.0
django-taggit==0.23.0
whoosh
```

## 虚拟环境

### 安装

```python
pip3 install virtualenv
```

### 创建环境

```python
virtualenv 环境名称
virtualenv Exercise
```

## pycharm配置

```python
打开步骤
File ==> settings ==> languages&frameworks ==> Django

Django project root: 项目路径
settings: 项目/settings.py

manage script: manage.py
    
run ==> eidt configurations
```



### 具体步骤

#### cmd指令

	D:
	cd D:\GitHub\Django
	virtualenv Django_drf
	
	激活
	d:
	cd D:\GitHub\Django\Django_drf\Scripts
	activate
	
	激活成功
	(Django_drf) D:\GitHub\Django\Django_drf\Scripts>
	
	退出
	d:
	cd D:\GitHub\Django\Django_drf\Scripts
	deactivate

## Django安装

在激活虚拟环境下安装
			(Django_drf) D:\GitHub\Django\Django_drf\Scripts>
				pip3 install django==1.11.29
			PyCharm安装的时候:
		注意不要勾选那个选项 (你们懂得)

		pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple/ django==1.11.29

## 创建项目

```python
django-admin startproject Django_drf
```

## Django项目的启动

#### 创建API

```python
python manage.py startapp api
```

## MySQL数据库编码问题

- ### 如django.db.utils.InternalError: (1366, "Incorrect string value: '\\xE6\\x96\\x87\\xE7\\xAB\\xA0...' for column 'name' at row 1")

- ### 在MySQL指令查询

```python
show variables like "%char%";
```

- ### 解决方案

```python
set character_set_database="utf8";
```

## 已存在数据库迁移

```python
python manage.py inspectdb > models.py
# managed = True
python manage.py migrate web --fake
python manage.py migrate
```

#### 创建超级账号密码

```python
python manage.py createsuperuser
```

#### 将类转换成数据表结构

```text
python manage.py makemigrations
```

#### 生成数据表

```python
python manage.py migrate
```

### pycharm

```Python
#快速指令
Ctrl+alt+R
导航栏-Tool-run manage.py T 
```



## MySQL时区配置

```
serverTimezon:Asia/Shanghai
```

- ### __ int __.py

- ```python
  # 固定写法
  import pymysql
  pymysql.install_as_MySQLdb()  # 告诉django用pymysql代替mysqldb连接数据库
  ```

- ### settings.py

```python
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
# 语言,时区
LANGUAGE_CODE = 'zh-hans'

TIME_ZONE = 'Asia/Shanghai'
```

## 命令行启动

#### 路径下

```python
D:\GitHub\Django\Django_drf\Django_drf
```

在项目的根目录下(也就是有manage.py的那个目录),运行:

```python
python manage.py runserver 127.0.0.1:99 --> 在指定的IP和端口启动
python3 manage.py runserver 端口   --> 在指定的端口启动
python3 manage.py runserver        --> 默认在本机的8000端口启动
#### 配置相关   
```



项目名/settings.py文件
		Templates(存放HTML文件的配置)       <-- 告诉Django去哪儿找我的HTML文件
		

```python
#静态文件(css/js/图片)
# 静态文件保存目录的别名
STATIC_URL = '/static/'
# STATIC_ROOT = os.path.join(BASE_DIR, 'static')

# 所有静态文件(css/js/图片)都放在我下面你配置的文件夹中
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static"),
]
# 上传文件
MEDIA_URL = '/uploads/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'uploads')
CKEDITOR_UPLOAD_PATH = 'allimg/'
#注释掉setting.py中 带有 csrf 的那一行(大概45~47行)
'django.middleware.csrf.CsrfViewMiddleware',
```

## pycharm过滤文件步骤

- File
  - Settings
    - project structure
      - exlcuded
