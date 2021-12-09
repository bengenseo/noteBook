# paramiko模块

## Django+paramiko

- ### 实现远程对服务器执行命令+上传下载文件

- #### 安装

  - ```python
    pip3 install paramiko
    ```

    ### 公钥和私钥（rsa）

    - #### 生成公钥和私钥

      ```
      ssh-keygen.exe -m pem
      
      在当前用户家目录会生成： .ssh/id_rsa.pub    .ssh/id_rsa
      ```

    - #### 把公钥放到服务器

      ```
      ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.16.85 
      ```

    - #### 以后再连接服务器时，不需要在输入密码

      ```
      ssh root@192.168.16.85
      ```

- ### 远程执行命令【用户名和密码】

  -  [paramiko远程执行命令.py](images\paramiko\paramiko远程执行命令.py) 

- ### 远程执行命令【公钥和私钥】(公钥必须提前上传到服务器)

  -  [私钥执行命令.py](images\paramiko\私钥执行命令.py) 

- ### 远程上传和下载文件【用户名和密码】

  -  [远程上传文件.py](images\paramiko\远程上传文件.py) 

- ### 远程上传和下载文件【公钥和私钥】

  -  [私钥上传下载文件.py](images\paramiko\私钥上传下载文件.py) 

- ### 通过私钥字符串也可以连接远程服务器。

  -  [私钥在内存.py](images\paramiko\私钥在内存.py) 

