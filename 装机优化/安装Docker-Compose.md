# 安装Docker-Compose

## 1.安装

```linux
# 下载安装包
# sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 国内速度慢-->可替换链接https://get.daocloud.io
sudo curl -L "https://get.daocloud.io/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
 
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



## 常用指令

```
# 后台启动
sudo docker-compose up -d
# 停止
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



