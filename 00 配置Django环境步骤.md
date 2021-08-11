# 配置步骤

## 版本依赖

```text
Django==1.11.29

django_redis==4.11.0

djangorestframework==3.9.0

腾讯云短信模块
tencentcloud-sdk-python
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

#### 创建表

```text
python manage.py migrate
```

#### 迁移表文件

```python
python manage.py makemigrations
```



## 命令行启动

#### 路径下

```python
D:\GitHub\Django\Django_drf\Django_drf
```

在项目的根目录下(也就是有manage.py的那个目录),运行:
				python manage.py runserver 127.0.0.1:99 --> 在指定的IP和端口启动
				python3 manage.py runserver 端口   --> 在指定的端口启动
				python3 manage.py runserver        --> 默认在本机的8000端口启动

#### 配置相关   

项目名/settings.py文件
		Templates(存放HTML文件的配置)       <-- 告诉Django去哪儿找我的HTML文件
		

```python
#静态文件(css/js/图片)
# 静态文件保存目录的别名
STATIC_URL = '/static/'

# 所有静态文件(css/js/图片)都放在我下面你配置的文件夹中
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static"),
]
#注释掉setting.py中 带有 csrf 的那一行(大概45~47行)
'django.middleware.csrf.CsrfViewMiddleware',
```