# django orm模板

## 模板

- ### 母版(basic.html)

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
  {% block header %}{% include 'header.html' %}{% endblock %}
  {% block content %}{% endblock %}
  {% block footer %}{% include 'footer.html' %}{% endblock %}
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

