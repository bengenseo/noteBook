# django orm模板

## 模板

### 母版(basic.html)

  ```html
  {% load static %}
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>{% block title %}{{ seotitle }}{{ title }}{% endblock %}</title>
      <meta name="keywords" content="{% block keywords %}{{ keywords }}{% endblock %}"/>
      <meta name="description" content="{% block description %}{{ description }}{% endblock %}"/>
      <meta property="og:url" content=""/>
      <meta property="og:image" content="logo.png"/>
      <meta property="og:site_name" content=""/>
      <meta property="og:title" content=""/>
      <meta property="og:description" content=""/>
      <meta name="generator" content="pycharm"/>
      <meta name="copyright" content="本站版权 Www.bengenseo.com 冰洁个人博客所有 All Rights Reserved"/>
      <link rel="stylesheet" href="{% static 'plugin/bootstrap/css/bootstrap.css' %}">
      <link rel="stylesheet" href="{% static 'plugin/font-awesome/css/font-awesome.min.css' %}">
      <link rel="shortcut icon" href="{% static 'favicon.ico' %}">
      {% block css %}{% endblock %}
  </head>
  <body>
  {% block header %}{% include 'header.htm' %}{% endblock %}
  {% block content %}{% endblock %}
  {% block footer %}{% include 'footer.htm' %}{% endblock %}
  <script src="{% static 'js/jquery-3.5.1.min.js' %}"></script>
  <script src="{% static 'plugin/bootstrap/js/bootstrap.min.js' %}"></script>
  {% block js %}{% endblock %}
  </body>
  </html>
  ```

## 调用模板

```html
{% extends 'basic.html' %}
{% load static %}
{% block title %}{% endblock %}
{% block keywords %}{% endblock %}
{% block description %}{% endblock %}
{% block css %}{% endblock %}
{% block content %}{% endblock %}
{% block js %}{% endblock %}
```

## 统计文章浏览次数

### models.py

```python
class Archives(models.Model):
    pass

    def increase_read_count(self):
        # 自增浏览量
        self.click += 1
        self.save(update_fields=['click'])
```

### views.py(即文章详情页: article.py)

```py
# 自增+1浏览数
    click = model.get(id=aid)
    click.increase_read_count()
```

## 反向生成URL

### urls.py

```python
# 声明当前app名
app_name='web'
urlpatterns = [
    # path('<int:article_id>.html', article.article, name='article'),  # 文章详情
    path('<slug:sitepath>/<int:article_id>.html', article.article, name='article'),  # 文章详情
]
```

### <slug:sitepath>中的slug和models.py中SlugField中匹配

### models.py

```python
class Archives(models.Model):
	sitepath = models.SlugField(verbose_name='栏目URL', max_length=20, null=True, blank=True)

    def get_absolute_url(self):  # 反向生成url
        # return reverse('web:article', kwargs={'article_id': self.id}) # 一个参数
        return reverse('web:article', kwargs={'sitepath': self.type.sitepath, 'article_id': self.id})  # 两个参数
```

### html,a链接

```html
{% for item in article %}
	{{ item.get_absolute_url }}
{% endfor %}
```

## 多层栏目菜单渲染

### html

### 神奇的标识符(“_set.all”);arctype为表单名

```html
<ul class="navbar-nav mr-auto">
    <li class="nav-item active">
        <a class="nav-link" href="/">首页</a>
    </li>
    {% for item in atype %}
    {% if item.topid == 1 and item.arctype_set.all %}
    <li class="nav-item dropdown">
        <a class="nav-link dropdown-toggle" href="{{ item.sitepath }}" id="navbarDropdown" role="button"
           data-toggle="dropdown"
           aria-expanded="false">
            {{ item.typename }}
        </a>
        <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
            {% for sub_item in item.arctype_set.all %}
            <li class="dropdown-item"><a href="{{  item.sitepath }}" class="d-block">{{ sub_item.typename }}</a>
            </li>
            {% endfor %}
            <li class="dropdown-divider"></li>
            <li class="dropdown-item"><a href="{{ item.sitepath }}" class="d-block">更多灯饰</a></li>
        </ul>
    </li>
    {% elif item.reid_id == None %}
    <li class="nav-item">
        <a class="nav-link" href="{{ item.sitepath }}">{{ item.typename }}</a>
    </li>
    {% endif %}
    {% endfor %}
</ul>
```











