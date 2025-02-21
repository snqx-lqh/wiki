## 简介

GitBook有网页版和本地版两种，网页版通过 https://www.gitbook.com 网址进行访问，本地版主要是基于Node.js 环境进行开发。本教程主要教大家Gitbook的本地版使用。

本项目内容基于松露老师的项目进行开发设计

松露老师：https://gitee.com/songlu-cube/courseware-gitbook-demo

[Gitbook操作指南——搭建个人电子书教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1dv411J7B8?p=1)

[GitBook 从懵逼到入门_人人都懂物联网-CSDN博客_gitbook](https://luhuadong.blog.csdn.net/article/details/81100704)

[Gitbook 超详细入门教程！电子书从入门到发布_qq_43363200的博客-CSDN博客](https://blog.csdn.net/qq_43363200/article/details/105506329)

本项目Gitee地址：[https://gitee.com/snqx-lqh/gitbook-operation-guide](https://gitee.com/snqx-lqh/gitbook-operation-guide)

book.js配置可以下载本项目配置,然后修改

## 配置Node.js环境

使用Gitbook需要配置Node.js环境，可以在[Node官网](https://nodejs.org/en/download/)进行下载，但是外网可能网速较慢，可以在国内镜像站，如[清华大学开源镜像站](https://mirrors.tuna.tsinghua.edu.cn/)进行搜索下载，记得下载较老的版本，新版本有部分问题，我安装的是10.23.0版本。下载链接([Index of /nodejs-release/v10.23.0/ | 清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/nodejs-release/v10.23.0/))，Windows版本下载.msi就可以了，安装步骤除了选择安装路径，其他都是下一步，可以网上找安装教程。

安装成功后执行命令可查看node版本和npm版本

```
# 查看node版本
node -v
# 查看npm版本
npm -v
```

## 安装Gitbook

使用下面命令，安装gitbook包

```
npm install -g gitbook-cli
```

## 初始化项目

### Gitbook初始化

创建一个文件夹，并在该文件夹下执行以下命令

```
gitbook init
```

执行结果

```
info: create SUMMARY.md
info: initialization is finished
```

可以看到创建了SUMMARY.md文档，这是电子书的目录文档。

然后创建一个REAMDE.md文档，用来对这个项目进行介绍。

### npm初始化

执行下面命令，初始化为npm项目。

```
npm init
```

命令会提示输入项目信息，可默认不填写，直接回车,但是记得第一个name信息必须是英文再下一步。

最后，会显示配置信息，输入yes回车即可初始化完毕。

初始化成功后，系统会自动在当前目录创建package.json文件，这是npm 项目的配置文件。
### 章节配置

GitBok使用文件SUMMARY.md来定义书本的章节和子章节的结构。文件SUMMARY.md被用来生成书本内容的预览表。

SUNMARY.md 的格式是一个简单的链接列表，链接的名字是章节的名字，链接的指向是章节文件的路径。

子章节被简单的定义为一个内嵌于父章节的列表。

```
- [简介](./README.md)

- [前言](./0前言/README.md)

- [第一章](./1第一章/README.md)

- [第二章](./2第二章/README.md)
  
  - [第二章 第1节](./2第二章/1第一节/README.md)
  - [第二章 第2节](./2第二章/2第二节/README.md)
```

## 启动项目

对于本地演示，我们可以直接通过下面命令启动。

```
gitbook serve
```

启动成功后，就可以在浏览器输入http:/ /localhost:4000/，如图所示。
## 忽略文件

任何在文件夹下的文件，在最后生成电子书时都会被拷贝到输出目录中，如果想要忽略某些文件，和Git一样，Gitbook会依次读取.gitignore , .bookignore和.ignore文件来将一些文件和目录排除。
## 配置文件

Gitbook在编译书籍的时候会读取书籍源码顶层目录中的 book.js 或者book.json，这里以 book.json为例，参考gibook文档可以知道，book.json常用的配置如下。

```
{
  //书籍信息
  "title": "Gitbook操作指南",
  "author": "snqx-lqh",
  "lang": "zh-cn",
  "description": "Gitbook电子书示例项目",
  
  //插件列表
  "plugins":[],
  //插件全局配置
  "pluginsConfig": {
  
  },
  //模板变量
  "variables": {
    
  },
}
```

当然，习惯用book.js 的同学也可以， book.js 只需要将JSON数据转为JS对象并导出即可，示例如下。

```
module.exports = {
  //书籍信息
  title: 'Gitbook操作指南',
  author: 'snqx-lqh',
  lang: 'zh-cn',
  description: 'Gitbook电子书示例项目',
  
  //插件列表
  plugins:[],
  //插件全局配置
  pluginsConfig: {
  
  },
  //模板变量
  variables: {
    
  },
};
```

## 安装编辑器

Giltbook编写可以用任何文本编辑器。在这里，我强烈安利Typora编辑器。Typora编辑器是非常非常非常好用的Markdown文件编器，直接进Typora官网下载对应平台的版本就可以了。当然，习惯用Visual Studio或者其他文本编辑工具的童鞋也可以根据自己的习惯自行选择。
## 插件

Gibook最灵活的地方就是有很多插件可以使用，当然如果对插件不满意，也可以自己写插件。所有插件的命名都是以gitbook-plugin-xxx的形式。下面，我们就介绍一些常用的插件。
### 搜索插件

在命令行输入下面命令安装搜索插件

```
npm install gitbook-plugin-search-pro
```

安装成功后，在book.js中添加插件的配置。

```
{
	plugins: ['search-pro'];
}
```

### 代码框插件

在命令行输入下面命令安装代码框架插件

```
npm install gitbook-plugin-code
```

安装成功后，在book.js中添加插件的配置。

```
{
	plugins: ['code'];
}
```

### 自定义主题插件

在命令行输入下面命令安装自定义主题插件

```
npm install gitbook-plugin-theme-主题名
```

安装成功后，在book.js中添加插件的配置。

```
{
	plugins: ['theme-主题名'];
}
```

### 菜单折叠插件

在命令行输入下面命令安装菜单折叠插件

```
npm install gitbook-plugin-expandable-chapters
```

安装成功后，在book.js中添加插件的配置。

```
{
	plugins: ['expandable-chapters'];
}
```
### 返回顶部插件

在命令行输入下面命令安装返回顶部插件

```
npm install gitbook-plugin-back-to-top-button
```

安装成功后，在book.js中添加插件的配置。

```
{
	plugins: ['back-to-top-button'];
}
```

更多插件可以从https://plugins.gitbook.com/获取。

## 构建项目

使用gitbook build构建项目，成功后即可在_book 文件夹中生成对应的静态资源。
