# Linux开pptpd

## 1、首先就是检查你VPS的PPP和TUN有没有启用，方法如下：

```linux
cat /dev/ppp
cat /dev/net/tun
```

显示结果为：cat: /dev/ppp: No such device or address和cat: /dev/net/tun: File descriptor in bad state，表明通过，上述两条只要有一个没通过都不行。如果没有启用，联系主机商让他开启。

## 安装iptables

```Linux
yum install -y ppp iptables
```

## 3、安装PPP

```text
yum install -y ppp
```

## 4、安装PPTPD

- ### 4.1、添加EPEL源：

```text
wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

- ### 4.2、安装EPEL源：

```text
rpm -ivh epel-release-latest-7.noarch.rpm
```

- ### 4.3、检查是否已添加到源列表中：

```text
yum repolist
```

- ### 4.4、更新源列表：

```text
yum -y update
```

- ### 4.5、安装PPTPD：

```text
yum install -y pptpd
```

## 5、编辑/etc/pptpd.conf设置内网IP段

最后IP设置改为：

```text
localip 192.168.0.1
remoteip 192.168.0.214,192.168.0.245
```

## 6、编辑/etc/ppp/options.pptpd

- ### 6.1、更改DNS项：

```text
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```

- ### 6.2、修改日志记录：

```text
nologfd
logfile /var/log/pptpd.log
```

## 7 、编辑/etc/ppp/chap-secrets设置PPTP账号密码

用户名 pptpd 密码 *//每个字段之间用tab键隔开 *表示用任意IP连接PPTP都可以

样例：登录账号为root 密码为123 这样写：

```text
test pptpd mylove@123 *
```

## 8 、编辑/etc/sysctl.conf修改内核参数支持内核转发

```text
net.ipv4.ip_forward=1
输入命令生效：sysctl -p
```

## 9、修改防火墙设置：

- ### 9.1、创建规则文件：touch /usr/lib/firewalld/services/pptpd.xml

- ### 9.2、修改规则文件

```text
<?xml version="1.0" encoding="utf-8"?>
<service>
       <short>pptpd</short>
       <description>PPTP</description>
       <port protocol="tcp" port="1723"/>
</service>
```

- ### 9.3 、启动或重启防火墙：

```text
systemctl start firewalld.service或firewall-cmd --reload
```

- ### 9.4、添加服务：

```text
firewall-cmd --permanent --zone=public --add-service=pptpd
```

- ### 9.5、允许防火墙伪装IP：

```text
firewall-cmd --add-masquerade
```

- ### 9.6、开启47及1723端口：

```text
firewall-cmd --permanent --zone=public --add-port=47/tcp
firewall-cmd --permanent --zone=public --add-port=1723/tcp
```

- ### 9.7、允许gre协议：

```text
firewall-cmd --permanent --direct --add-rule ipv4 filter INPUT 0 -p gre -j ACCEPT
firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 0 -p gre -j ACCEPT
```

- ### 9.8、设置规则允许数据包由eth0和ppp+接口中进出

```text
firewall-cmd --permanent --direct --add-rule ipv4 filter FORWARD 0 -i ppp+ -o eth0 -j ACCEPT
firewall-cmd --permanent --direct --add-rule ipv4 filter FORWARD 0 -i eth0 -o ppp+ -j ACCEPT
```

- ### 9.9 、设置转发规则，从源地址发出的所有包都进行伪装，改变地址，由eth0发出：

```text
firewall-cmd --permanent --direct --passthrough ipv4 -t nat -I POSTROUTING -o eth0 -j MASQUERADE -s 192.168.0.0/24
```

## 10、重启服务器：

```text
firewall-cmd --reload
systemctl restart pptpd
```