# pycharm主题配置

## Django颜色高亮

![image-20230602014920671](./assets/image-20230602014920671.png)

## Django代码块

![image-20230602021534068](./assets/image-20230602021534068.png)

```py
{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}{{ title }}{% endblock %}</title>
    <meta name="keywords" content="{% block keywords %}{{ keywords }}{% endblock %}"/>
    <meta name="description" content="{% block description %}{{ description }}{% endblock %}"/>
    <meta property="og:url" content=""/>
    <meta property="og:image" content="logo.png"/>
    <meta property="og:site_name" content=""/>
    <meta property="og:title" content=""/>
    <meta property="og:description" content=""/>
    <meta name="generator" content="pycharm"/>
    <meta name="copyright" content="本站版权 Www.bengenseo.com 冰洁个人博客所有 All Rights Reserved"/>
    <link rel="stylesheet" href="{{request.build_absolute_uri}}{% static '' %}">
    <link rel="stylesheet" href="{{request.build_absolute_uri}}{% static '' %}">
    <link rel="shortcut icon" href="{% static 'favicon.ico' %}">
    <script src="{{request.build_absolute_uri}}{% static '' %}"></script>
    <script src="{{request.build_absolute_uri}}{% static '' %}"></script>
    {% block css %}
    
    {% endblock %}
</head>
<body>
{% block header %}{% include 'header.htm' %}{% endblock %}
{% block body %}
$END$
{% endblock %}
{% block footer %}{% include 'footer.htm' %}{% endblock %}

{% block js %}{% endblock %}
</body>
</html>
```

```py
{% $END$ %}

{% extends "basic.html" %}

{% block body %}
$END$
{% endblock %}

{% block css %}$END${% endblock %}

{% block description %}$END${% endblock %}

{% block footer %}
$END$
{% endblock %}

{% block header %}
$END$
{% endblock %}

{% include '$END$' %}

{% block js %}
$END$
{% endblock %}

{% block keywords %}$END${% endblock %}

{% spaceless %}去除html空白符
$END$
{% endspaceless %}

{% static '$END$' %}

{% block title %}$END${% endblock %}

{% csrf_token %}

{{request.build_absolute_uri}}
```

```py
{% extends "basic.html" %}
{% load static %}
{% block title %}{% endblock %}
{% block keywords %}{% endblock %}
{% block description %}{% endblock %}
{% block css %}

{% endblock %}


{% block body %}
$END$
{% endblock %}


{% block footer %}

{% endblock %}


{% block js %}

{% endblock %}
```

## HTML颜色高亮

![image-20230602015051844](./assets/image-20230602015051844.png)