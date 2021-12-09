#  CentOS指令

## centos7指令

### 网关

```text
ifconfig
ip  
```

### 历史指令

```text
history
```

### 指令模式

```text
systemctl set-default graphical.target
# 多用户
systemctl set-default multi-user.target
```

### 桌面模式

```text
startx
```

## 指令

- ### 重启防火墙
  - ```text
    systemctl restart firewalld.service
    ```

- ### 打开单个端口

  - ```text
    firewall-cmd --zone=public --add-port=9956/tcp --permanent
    ```

- ### 查看开启端口

  - ```text
    netstat -ntlp
    firewall-cmd --list-ports
    ```

- ### 关闭端口命令
  - ```text
    firewall-cmd --zone=public --remove-port=8888/tcp --permanent
    ```

- ### 重启

  - ```text
    reboot
    ```

- ### 宝塔

  - ```text
    http://119.91.124.67:9956/5a27ca60
    ah04acfm
    p4#RnxdYt@
    ```

- #### 账号密码

  - ```text
    T7cXmPwN5
    ```

- ### 服务器密码

  - ```text
    cY$0m5rSAPJ5cG5o8Suz
    ```
