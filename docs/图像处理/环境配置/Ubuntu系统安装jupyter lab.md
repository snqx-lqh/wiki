## 安装jupyterlab

```BASH
pip install jupyterlab
pip install jupyterlab-language-pack-zh-CN
```

## 生成配置

生成jupyterlab的相关配置

```BASH
jupyter lab --generate-config
```

上面命令会生成`jupyterlab`配置文件，路径为`~/.jupyter/jupyter_lab_config.py`

## 创建密码

```BASH
jupyter lab password
```

填写密码并确认密码，会生成`~/.jupyter/jupyter_server_config.json`，查看文件内容，并复制`password`后的一串字符

## 修改配置文件

```BASH
vim ~/.jupyter/jupyter_lab_config.py
```

在配置中，设置以下内容

```BASH
# 将ip设置为*，意味允许任何IP访问
c.NotebookApp.ip = '*'
# 这里的密码就是上边我们生成的那一串
c.NotebookApp.password = '刚复制的字符串'
# 服务器上并没有浏览器可以供Jupyter打开 
c.NotebookApp.open_browser = False 
# 监听端口设置为8888或其他自己喜欢的端口 
c.NotebookApp.port = 8888
# 允许远程访问 
c.NotebookApp.allow_remote_access = True 
```

## 运行

终端上运行：

```BASH
jupyter lab --allow-root
```

后台运行：

```BASH
nohup jupyter lab --allow-root &
```

删除运行

```BASH
# 查看运行的进程号
lsof -i:8701
# 删除对应的进程
kill -9 1907
```