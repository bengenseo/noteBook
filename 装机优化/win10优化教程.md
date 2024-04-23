# windows 10 优化教程

## 删除IE

### 快捷键: win+R

```text
regedit
```

### 路径

```text
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Desktop\NameSpace\
```



### IE注册表为

```text
{B416D21B-3B22-B6D4-BBD3-BBD452DB3D5B5}
```



## 虚拟机VMware

### 首先安装VMware tools工具

```text
导航栏-->虚拟机-->VMware Tools
```



## 删除默认7个文件夹

### 路径

```text
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace
```

### 说明

```text
{088e3905-0323-4b02-9826-5d99428e115f}→下载文件夹；

{0DB7E03F-FC29-4DC6-9020-FF41B59E513A}→3D对象文件夹；

{24ad3ad4-a569-4530-98e1-ab02f9417aa8}→图片文件夹；

{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}→桌面文件夹；

{d3162b92-9365-467a-956b-92703aca08af}→文档文件夹；

{f86fa3ab-70d2-4fc7-9c99-fcbf05467f3a}→视频文件夹；

{3dfdf296-dbec-4fb4-81d1-6a3438bcf4de}→音乐文件夹；
```



### 2.文件一键删除(文件另存为reg格式,**否则可能失败**)

```text
Windows Registry Editor Version 5.00

[-HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\{f86fa3ab-70d2-4fc7-9c99-fcbf05467f3a}]

[-HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\{d3162b92-9365-467a-956b-92703aca08af}]

[-HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}]

[-HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\{3dfdf296-dbec-4fb4-81d1-6a3438bcf4de}]

[-HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\{088e3905-0323-4b02-9826-5d99428e115f}]

[-HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\{0DB7E03F-FC29-4DC6-9020-FF41B59E513A}]

[-HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\{24ad3ad4-a569-4530-98e1-ab02f9417aa8}]
```

##### 	弹出一个对话框,点击-->是

##### 	运行文件



## Windows 10登录或登录屏幕背景图片默认为模糊

### 一.开启透明效果

```text
桌面空白处-->右键-->个性化-->颜色-->透明效果
```

### 二.通过组策略禁用登录屏幕模糊

```text
1、在“开始/任务栏搜索”字段中键入“编辑组策略”，然后按Enter键，打开“组策略编辑器” 。

2、导航至：计算机配置 > 管理模板 > 系统 > 登录

3、在右侧，查找“显示清楚的登录背景”，双击它以打开其属性。

4、选择“已启用”选项，然后单击“应用”按钮关闭登录屏幕背景图片中的模糊效果并显示清晰的图片。
```

### 三.通过注册表开启登录屏幕模糊

#### 步骤

```text
在开始/任务栏搜索栏中键入“注册表编辑”，然后按回车键，当你获得用户帐户控制屏幕以启动注册表编辑器时，请单击“是”按钮。
```

##### 路径

```text
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\LogonUI\TestHooks
```

##### 数值

```text
1.如果没有二进制
2.创建名称为
DisableAcrylicBackgroundOnLogon
3.数值
	0为打开模糊
	1为关闭模糊
```

## Windows 10任务栏完全透明

### **注意**:个性化-颜色-以下区域主题颜色(不能打勾)

### 一.Windows+R打开运行，输入regedit回车打开注册表。
二.路径

```text
计算机\HKEY_CURRENT_USER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced
```

### 三.手动添加一个（类型为DWORD）命名为

```text
TaskbarAcrylicOpacity
```

#### 	赋值0

### 四.重启文件资源管理器生效

### 五.毛玻璃的修复教程

```text
桌面-鼠标右键-个性化-颜色-将“选择颜色”的浅色切换成深色(或将深色切换成浅色再切换成深色)-透明效果开启(或关闭后开启), 即可
```

## Win10系统频繁弹出在Microsoft Store中查应用怎么解决？

```text
　1、首先，按Win+R组合快捷键，在打开的运行窗口中输入regedit，点击确定或按回车即可;


　　2、然后，在进入注册表编辑器，定位到：

　　HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer。


　　注：如果没有Explorer这项的话，就新建一个;

　　3、在其右侧新建个名为 NoUseStoreOpenWith 的DWORD（32位）值，把数值数据设置成1，最后再点击确定保存设置即可。
```

## 恢复默认DNS：

打开命令提示符或 Windows PowerShell，键入ipconfig /flushdns，然后按 Enter。

