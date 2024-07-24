# RouterOS配置

## 重置系统操作

### 断电->按住按钮->通电->直到灯闪烁才成功

![image-20240722012241560](./assets/image-20240722012241560.png)

## 查看型号

https://mikrotik.com/products

## 打开winbox

https://mikrotik.com/download

![image-20240722012633144](./assets/image-20240722012633144.png)

### 密码空

### 可设置密码

![image-20240722012701773](./assets/image-20240722012701773.png)

## 先清除默认配置

![image-20240722063625218](./assets/image-20240722063625218.png)

## DHCP连接

![image-20240722012938109](./assets/image-20240722012938109.png)

## 类似光猫,PPPOE拨号(DHCP二选一)

![image-20240722013324639](./assets/image-20240722013324639.png)

## 主要核对端口和重命名

![image-20240722013534549](./assets/image-20240722013534549.png)

## 创建wifi密码

![image-20240722015325940](./assets/image-20240722015325940.png)

## 创建wifi

### mode要选 ap bridge(ap桥接)

![image-20240722014119657](./assets/image-20240722014119657.png)

## 创建桥接

![image-20240722015446558](./assets/image-20240722015446558.png)

## 通网自动桥接

![image-20240722015815108](./assets/image-20240722015815108.png)

### 创建后,进行关闭,不想和lan口串联

![image-20240722015948317](./assets/image-20240722015948317.png)

### 一次性获取全部接口或wifi,如果个别定制,请先创建,然后再all.因为all后,不允许更改.

![image-20240722020239434](./assets/image-20240722020239434.png)

## 创建代理IP

![image-20240722020606117](./assets/image-20240722020606117.png)

## 创建网关

![image-20240722020805709](./assets/image-20240722020805709.png)

## 获取上游(光猫)IP

![image-20240722021032503](./assets/image-20240722021032503.png)

## 创建IP池

![image-20240722021412497](./assets/image-20240722021412497.png)

## 自动分配IP

![image-20240722021536804](./assets/image-20240722021536804.png)

### 广播网络

![image-20240722021634031](./assets/image-20240722021634031.png)

## 添加DNS

![image-20240722021833197](./assets/image-20240722021833197.png)

## 设置防火墙

![image-20240722022233905](./assets/image-20240722022233905.png)

## IP线路

![image-20240722022909747](./assets/image-20240722022909747.png)

## 最安全,只能本地winbox.exe连接设置ROS系统

![image-20240722023224647](./assets/image-20240722023224647.png)

### 其他都关掉

## IP分流代理

### Linux运行

```
curl -s https://raw.githubusercontent.com/17mon/china_ip_list/master/china_ip_list.txt |sed -e 's/^/add address=/g' -e 's/$/ list=CNIP/g'|sed -e $'1i\\\n/ip firewall address-list' -e $'1i\\\nremove [/ip firewall address-list find list=CNIP]' -e $'1i\\\nadd address=192.168.88.0/24 list=CNIP comment=private-network'>cnip.rsc
```

### cnip.rsc文件拖入winbox

### CMD终端窗口运行

### 如果ros默认没密码

### 使用终端需要先设置密码,new password输入两次密码

### 再运行

```
import cnip.rsc
```

### 成功返回

```
Script file loaded and executed successfully
```

