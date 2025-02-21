
## Remote - SSH使用

### 安装

安装插件，插件名称就是 Remote - SSH

### 配置SSH密钥

将本机添加到远程服务器连接白名单，让服务器知道是已认证的电脑在连接

使用如下命令，生成 SSH 公钥文件。

```bash
ssh-keygen
```

会让输入保存路径，默认即可。我的生成之后在`C:\Users\LQH\.ssh`路径下

找到公钥文件 `id_rsa.pub` ，复制到远程服务器 **根目录** 的 `.ssh` 文件夹中。

（1）根目录，不一定非要是 `/.ssh` 路径，可以是自己的用户目录，类似这样：`/home/lqh/.ssh`。

（2）`.ssh` 文件夹没有怎么办？新建一个文件夹，命名为 `.ssh` 即可。同时要确认远程服务器是否支持 SSH ，如果此时正是通过 SSH 方式连接的，那肯定是支持了。

3、生成 `authorized_keys` 文件。这样后续在使用 Remote 插件时，不需要密码，就可以直接登录到服务器。

进入 服务器`.ssh` 目录，使用如下命令，生成 `authorized_keys` 文件。`id_rsa.pub`是前面说的在windows下生成的

```bash
cat id_rsa.pub > authorized_keys
```

### 添加配置文件

目的：**配置 VSCode 连接远程服务器的一些基本信息。**

1、点击左侧的 “远程资源管理器” 图标，点击右上角的小齿轮（设置）

2、在弹出来的窗口中，选择第一个 config 文件打开，填写对应信息，例如

```bash
Host <远程主机名称>
    HostName <远程主机IP>
    User <用户名>
    Port <ssh端口，默认22>
    IdentityFile <本机SSH私钥路径>
    ForwardAgent yes <VSCode 自己添加的，不用管>

Host ubuntu18
    HostName 192.168.92.132
    User lqh
    Port 22
    IdentityFile "C:\Users\LQH\.ssh\id_rsa"
    ForwardAgent yes
```