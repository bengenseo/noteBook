# docker compose教程

## 安装docker

### 更新包

```
yum -y update
```

### docker卸载旧版

```
yum remove docker  docker-common docker-selinux docker-engine
```

### 驱动源

```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

### 查看版本

```
yum list docker-ce --showduplicates | sort -r
```

### 版本

```
yum -y install docker-ce-18.03.1.ce
```

### 开机启动

```
systemctl start docker
systemctl enable docker
```

### 关闭防火墙

```
systemctl stop firewalld
systemctl disable firewalld
```

### 常用命令(启动,停止,重启)

```
systemctl start docker
systemctl stop docker
systemctl restart docker
```

### 国内镜像加速源

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://idsc5w8g.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 配置信任地址

```
# 打开要修改的文件
vi /etc/docker/daemon.json
# 添加内容：
"insecure-registries":["http://192.168.200.105:8080"]
systemctl daemon-reload
systemctl restart docker
```

## docker compose

### 安装

```
curl -L https://github.com/docker/compose/releases/download/v2.20.3/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

### 文件权限

```
chmod +x /usr/local/bin/docker-compose
```











