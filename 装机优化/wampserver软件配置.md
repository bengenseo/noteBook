# wampserver配置

## 修改根目录

### **1、修改wampserver的默认目录**

安装好wampserver后，网站根目录默认为：安装目录\wamp\www，也就是wampserver安装目录下的www文件夹。

我们以更改为:D\www为例。 打开wamp\scripts\config.inc.php

第47行，$wwwDir =$c_installDir.’/www’;（ $c_installDir是个变量，指WAMPserver安装根目录。）

修改为：$wwwDir ='D:/www';即可。

第2步

### **2、修改Apache默认根目录**

打开wamp\bin\apache\apache2.2.11\conf\httpd.conf,修改DocumentRoot后面双引号中的值为你所要的。

比如将DocumentRoot “C:/wamp/www/” 改成DocumentRoot “D:/www/”

同时将<Directory “E:/wamp/www/“> 改成<Directory “E:/www/“>



## 配置域名

### hosts

### 路径:

```text
C:\Windows\System32\drivers\etc\hosts
```

### 文件:

```text
192.168.200.131 start.dnf.tw
192.168.10.131 start.dnf.tw
127.0.0.1 localhost
::1 localhost
127.0.0.1 php.com
127.0.0.1 demo.com
127.0.0.1 demo1.com
127.0.0.1 demo2.com
127.0.0.1 demo3.com
127.0.0.1 demo4.com
127.0.0.1 demo5.com
127.0.0.1 web.com
127.0.0.1 business.com
192.168.1.4 a.com
127.0.0.1 b.com
127.0.0.1 c.com
127.0.0.1 d.com
127.0.0.1 e.com
127.0.0.1 f.com
127.0.0.1 g.com
127.0.0.1 h.com
127.0.0.1 i.com
127.0.0.1 j.com
127.0.0.1 k.com
127.0.0.1 l.com
127.0.0.1 m.com
127.0.0.1 n.com
127.0.0.1 o.com
127.0.0.1 p.com
127.0.0.1 q.com

#配置cmswing的域名
127.0.0.1 wing.cms

127.0.0.1 www.xmind.net
#0.0.0.0 account.jetbrains.com
#0.0.0.0 www.jetbrains.com

127.0.0.1 activate.navicat.com

# Added by Docker Desktop
192.168.1.4 host.docker.internal
192.168.1.4 gateway.docker.internal
# To allow the same kube context to work on the host and the container:
127.0.0.1 kubernetes.docker.internal
# End of section
```

## httpd-vhosts.conf

### 路径

```text
\wamp64\bin\apache\apache2.4.39\conf\extra
```

### 文件

```text
# Virtual Hosts
#
<VirtualHost *:80>
  ServerName localhost
  ServerAlias localhost
  DocumentRoot "${INSTALL_DIR}/www"
  <Directory "${INSTALL_DIR}/www/">
    Options +Indexes +Includes +FollowSymLinks +MultiViews
    AllowOverride All
    Require local
  </Directory>
</VirtualHost>

<VirtualHost *:80>
  ServerName web.com
  ServerAlias web.com
  DocumentRoot "C:/body/web"
  <Directory "C:/body/web">
    Options +Indexes +Includes +FollowSymLinks +MultiViews
    AllowOverride All
    Require local
  </Directory>
</VirtualHost>

<VirtualHost *:80>
  ServerName php.com
  ServerAlias php.com
  DocumentRoot "C:/body/php"
  <Directory "C:/body/php">
    Options +Indexes +Includes +FollowSymLinks +MultiViews
    AllowOverride All
    Require local
  </Directory>
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/demo"
 ServerName demo.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/demo1"
 ServerName demo1.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/demo2"
 ServerName demo2.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/demo3"
 ServerName demo3.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/demo4"
 ServerName demo4.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/demo5"
 ServerName demo5.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/business"
 ServerName business.com
</VirtualHost>

<VirtualHost *:80>
  ServerName 192.168.1.4
  ServerAlias 115.218.74.138
  DocumentRoot "C:/body/a"
  <Directory "C:/body/a">
    Options +Indexes +Includes +FollowSymLinks +MultiViews
    AllowOverride All
    Require all granted
  </Directory>
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/b"
 ServerName b.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/c"
 ServerName c.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/d"
 ServerName d.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/e"
 ServerName e.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/f"
 ServerName f.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/g"
 ServerName g.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/h"
 ServerName h.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/i"
 ServerName i.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/j"
 ServerName j.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/k"
 ServerName k.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/l"
 ServerName l.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/m"
 ServerName m.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/n"
 ServerName n.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/o"
 ServerName o.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/p"
 ServerName p.com
</VirtualHost>

<VirtualHost *:80>
 DocumentRoot "C:/body/q"
 ServerName q.com
</VirtualHost>
```

