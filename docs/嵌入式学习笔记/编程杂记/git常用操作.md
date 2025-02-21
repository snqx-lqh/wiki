## 下载与安装

软件下载地址：

https://gitforwindows.org/

安装过程比较简单，只需要下一步就好，注意安装路径，尽量不要有中文。

安装完成后打开cmd，输入git --version，如果能查到版本号，就是安装成功了。

## 安装完成后的初始操作

刚安装完成的时候需要配置一些初始文件

```bash
//初始化配置
$ git config --global user.name "你的用户名"
$ git config --global user.email "你的邮箱"
```

## 基本操作

在要管理的文件夹下右键打开Git Bash Here，然后

```bash
git init
```

添加文件到暂存区

```bash
git add <filename> //单个文件
git add .          //全部文件
```

把暂存区的文件提交到仓库

```bash
git commit -m "提交信息"
```

查看提交的历史记录

```bash
git log --stat
```

查看现在文件状态

```bash
git status
```

给远程仓库起别名

```bash
git remote add <别名> <远程仓库地址>
example: git remote add origin https://github.com/paulboone/ticgit
```

推送本地代码到远程仓库

```bash
git push -u <地址> <远程分支名> 
git push -u origin master
git push -u origin master --force  强行推送
```

如果远程仓库有代码，第一次要将远程仓库的文件拉取下来

```bash
git pull --rebase origin master
```

远程仓库克隆

```bash
git clone <git地址>
```

推送当前分支最新的提交到远程

```bash
git push
```

拉取远程分支最新的提交到本地

```bash
git pull
```

## 分支操作

以当前分支为基础新建分支

```bash
git checkout -b <branchname>
```

列举所有的分支

```bash
git branch
```

单纯地切换到某个分支

```bash
git checkout <branchname>
```

删掉特定的分支

```bash
git branch -D <branchname>
```

合并分支

```bash
git merge <branchname>
```

## 其他操作

撤销最后一次提交

```bash
git reset HEAD^1
```

配置代理，1080就是代理的端口号

```bash
git config --global http.proxy socks5://127.0.0.1:7890 
git config --global https.proxy socks5://127.0.0.1:7890
```

强制覆盖本地

```BASH
git fetch --all
git reset --hard origin/master
```

git报错LF

```BASH
git config --global core.autocrlf false
```

## 使用VScode版本控制

关于这个，可以看B站冯雨up的经验分享：[40 分钟学会 Git | 日常开发全程大放送&搭配Github_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1db4y1d79C?from=search&seid=6339066584482688996&spm_id_from=333.337.0.0)

下面是他的视频补充，遇到有相关问题时注意查看

>补充1：有些朋友点开终端后默认打开的是 powershell 而不是 bash，请按照如图这样操作：https://img.alicdn.com/imgextra/i1/O1CN01vHIh9o21axwCs22dr_!!6000000007002-2-tps-2102-454.png
>
>补充2：有些朋友点开分支管理发现没有 file histroy/commits 之类的增强功能，我疏忽了，这个需要安装一个 vscode 必备插件 Gitlens，安装方法如图：https://img.alicdn.com/imgextra/i3/O1CN01JxtnGQ1gd9u3aZ3JZ_!!6000000004164-2-tps-2880-1750.png
>
>补充3：有些朋友安装 git 到了一个自定义的目录导致 vscode 找不到终端的位置所以没有bash，重新安装到默认位置即可。