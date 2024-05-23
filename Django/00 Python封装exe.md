# 00 Python封装可执行文件exe

```
pip install pyinstaller
```



## 隐藏cmd窗口

```
-F或--onefile(一个文件)
-w 或 --windowed(cmd窗口)
```

```python
 pyinstaller -F -w cms.py
```

## 正常打包

```python
 pyinstaller -F cms.py
```

## ico

```
pyinstaller --icon=auto.ico cms.py
```

# 如一个exe文件运行失败

## 文件缺失,需要以下步骤

## text.spec

### text.spec需要修改的部分

```

a = Analysis(
    datas=[('C:\\Users\\74799\\Desktop\\Project\\.venv\\Lib\\site-packages\\whisper', './whisper')],
)
exe = EXE(
    console=True,
    icon=['auto.ico'],
)

```

### 命令行

```
pyinstaller  text.spec
```

## text.py

### 需要打包的代码exe

### 如果没有 `freeze_support` ，它只会无限循环。

```
from multiprocessing import freeze_support
def adi2txt():
	pass
if __name__ == '__main__':
    freeze_support()
    adi2txt()
```

