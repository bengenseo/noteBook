# openwrt配置

## DNS设置(问题根源)

### 路径/etc/config/network

### 一定要加DNS

```
config interface 'lan'
	option device 'br-lan'
	option proto 'static'
	option ipaddr '192.168.200.103'
	option netmask '255.255.255.0'
	option ip6assign '60'
	option gateway '192.168.200.101'
    option dns '223.5.5.5 223.6.6.6 8.8.8.8 8.8.4.4 114.114.114.114'
```



## 镜像源

清华大学：https://mirrors.tuna.tsinghua.edu.cn/openwrt

中科大：https://mirrors.ustc.edu.cn/openwrt

腾讯：https://mirrors.cloud.tencent.com/openwrt

/etc/opkg/distfeeds.conf

```
src/gz openwrt_core https://mirrors.cloud.tencent.com/openwrt/releases/22.03.6/targets/x86/64/packages
src/gz openwrt_base https://mirrors.cloud.tencent.com/openwrt/releases/22.03.6/packages/x86_64/base
src/gz openwrt_luci https://mirrors.cloud.tencent.com/openwrt/releases/22.03.6/packages/x86_64/luci
src/gz openwrt_packages https://mirrors.cloud.tencent.com/openwrt/releases/22.03.6/packages/x86_64/packages
src/gz openwrt_routing https://mirrors.cloud.tencent.com/openwrt/releases/22.03.6/packages/x86_64/routing
src/gz openwrt_telephony https://mirrors.cloud.tencent.com/openwrt/releases/22.03.6/packages/x86_64/telephony
```

### 文档 https://v2raya.org/docs/prologue/installation/openwrt/

## 手动安装 V2Ray

### 1.v2ray-linux-64.zip放在/root/

```
https://github.com/v2fly/v2ray-core/releases/download/v4.40.1/v2ray-linux-64.zip
```

### 2.连不了外网不执行

```
wget https://github.com/v2fly/v2ray-core/releases/download/v4.40.1/v2ray-linux-64.zip
```

### 只执行

```
opkg update; opkg install unzip wget-ssl
unzip -d v2ray-core v2ray-linux-64.zip
cp v2ray-core/v2ray v2ray-core/v2ctl /usr/bin
chmod +x /usr/bin/v2ray; chmod +x /usr/bin/v2ctl
```

### OpenWrt 22.03 或更新版本：

```bash
opkg update
opkg install \
    ca-bundle \
    ip-full \
    kmod-nft-tproxy
```

## 下载二进制

```
https://github.com/v2rayA/v2rayA/releases/download/v2.0.5/v2raya_linux_x64_2.0.5
```

### 命名为v2raya

### 路径/usr/bin/

```
/usr/bin/v2raya && chmod +x /usr/bin/v2raya
```

#### 下载配置文件和服务文件

```
wget https://raw.githubusercontent.com/openwrt/packages/master/net/v2raya/files/v2raya.config -O /etc/config/v2raya
wget https://raw.githubusercontent.com/openwrt/packages/master/net/v2raya/files/v2raya.init -O /etc/init.d/v2raya
chmod +x /etc/init.d/v2raya
```

## 启动

```
uci set v2raya.config.enabled='1'
uci commit v2raya
/etc/init.d/v2raya enable
/etc/init.d/v2raya start
```

## 使用说明

端口是2017

不是http://localhost:2017

openwrt真实IP如:192.168.200.103:2017

https://v2raya.org/docs/prologue/quick-start/

## 查找软件包

```
opkg list_installed
```

## 删除连同依赖

```
opkg remove 插件名 with --force-removal-of-dependent-packages
```

