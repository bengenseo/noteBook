# 安装Docker-Compose

## 清空cmd行

```
clear
```

## 安装Python

https://blog.csdn.net/weixin_44874451/article/details/128363789

```
#添加软连接
sudo ln -s /usr/local/bin/python39
sudo ln -s /usr/local/bin/python39/bin/pip3
```

```
cd root/.virtualenvs/blog/bin/activate
```

## linux删除文件

```
#删除文件
rm -fr 文件名
#查看所有,包括隐藏文件
ls -a
```





## 1.安装

```linux
# 下载安装包
curl -SL https://github.com/docker/compose/releases/download/v2.19.1/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
# 国内速度慢-->可替换链接https://get.daocloud.io
sudo curl -L "https://get.daocloud.io/docker/compose/releases/download/1.29.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
 
# 设置权限，应用可执行权限
sudo chmod +x /usr/local/bin/docker-compose
# 添加软连接
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
# 查看安装信息
docker-compose --version
```

## 或2.pip安装

```
# 1. 安装python-pip
yum -y install epel-release
yum -y install python-pip
# 2. 安装docker-compose
pip install docker-compose
# 3. 待安装完成后，执行查询版本的命令，即可安装docker-compose
docker-compose version
```

## yml文件

```
version: "3.8"
services:
  web:
    build: .
    volumes:
      - .:/django/blog
    ports:
      - 80:80
    image: web:django
    container_name: blog_container
    command: python manage.py runserver 0.0.0.0:80
    networks:
      - proxy

  mysql:
    image: mysql:8.0
    restart: always #出现错误就重启
    container_name: mysql
    privileged: true #root外部只是个普通用户
    environment:
      #MYSQL_ROOT_PASSWORD: root#超级管理员
      MYSQL_DATABASE: blog #数据库名
      #自定义数据库的用户
      MYSQL_USER: blog
      MYSQL_PASSWORD: secret
    command:
      --default-authentication-plugin=mysql_native_password
      --character-set-server=utf8mb4
      --collation-server=utf8mb4_0900_ai_ci
      --explicit_defaults_for_timestamp=true
      --lower_case_table_names=1
      --max_allowed_packet=128M;
    ports:
      - 3306:3306
    volumes:
      - /usr/local/bin/mysql:/var/lib/mysql
      - /etc/localtime:/etc/localtime:ro
networks:
  proxy:
    #external: #参考外部
      name: blog_network
```

## 删除宿主机文件

```
rm -rf /usr/local/bin/mysql
```

## 常用指令

```
# 后台启动
sudo docker compose up -d
#精灵线程 检测yml文件
docker compose up -d --build
# 删除
sudo docker-compose down
# 查询容器列表
sudo docker-compose ps
# 查询日志 (查询所有日志，可以辅助排查个别容器启动失败问题)
sudo docker-compose logs
# 查看版本
sudo docker-compose version 
```



## 卸载

```
# 卸载数据
sudo rm /usr/local/bin/docker-compose
```





