# pycharm远程配置

## 文件对齐格式化

```text
快捷键 Ctrl+Alt+L
```



## 权限设置

### 受限

```text
ii_env\Scripts\activate : 无法加载文件 F:\gitlab\AutoFrame\ii_env\Scripts\Activate.ps1，因为在此系统上禁止运行脚本。
有关详细信息，请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 1

ii_env\Scripts\activate
  + CategoryInfo          : SecurityError: (:) []，PSSecurityException+ FullyQualifiedErrorId : UnauthorizedAccess
```

### 打开权限

![](./assets/1.png)

![](./assets/2.png)

### 代码命令:

```cmd
Get-ExecutionPolicy
Set-ExecutionPolicy Bypass
```

### 说明:

```text
以管理员身份运行PowerShell

在PowerShell里面执行命令：Get-ExecutionPolicy

输出的是：Restricted

表明当前是严格受限模式，需要设置打开，在PowerShell里面执行命令：Set-ExecutionPolicy Bypass
设置打开后，在PowerShell里面执行命令：Get-ExecutionPolicy

输出的是：Bypass

设置完成后，再次重启Pycharm，再次运行ii_env\Scripts\activate不会报错，可进入到虚拟环境
```

## 背景图

![](./assets/image-20220909084319449.png)

## 代码提示

### 插件:kite

## 快速导入模块

![](./assets/image-20211102215701307.png)

### 快捷键:

```python
alt+enter
```



## 全局引入模块

![](./assets/image-20211102220013040.png)

### 局部引入模块

![](./assets/image-20211102220154459.png)

## 提交GitHub配置

![image-20211108204245276](./assets/image-20211108204245276.png)

![image-20211108204324643](./assets/image-20211108204324643.png)

![image-20211108204417366](./assets/image-20211108204417366.png)

![image-20211108204451701](./assets/image-20211108204451701.png)

## pycharm配置

![image-20210903232713741](./assets/image-20210903232713741.png)

![image-20210903235059936](./assets/image-20210903235059936.png)

## 步骤返回

![image-20210912202144393](./assets/image-20210912202144393.png)



![image-20210912202223576](./assets/image-20210912202223576.png)

## 快速提示未导入的依赖包安装

![image-20220506044136622](./assets/image-20220506044136622.png)

![image-20220506052525147](./assets/image-20220506052525147.png)

## 修改镜像源

```py
阿里云 
http://mirrors.aliyun.com/pypi/simple/
豆瓣(douban) 
http://pypi.douban.com/simple/

```

