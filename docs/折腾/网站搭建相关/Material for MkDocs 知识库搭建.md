## 项目配置说明

这是一个基于MkDocs框架，Material主题的知识库搭建流程，主要参考教程就是Material的官方教程。[链接](https://squidfunk.github.io/mkdocs-material/getting-started/)

可以直接阅读文档，我这里讲述我的配置流程。

我的Wiki主页：https://snqx-lqh.github.io/wiki/

Cloudflare镜像站：https://wiki-20f.pages.dev/
## Miniconda环境搭建

由于MkDocs框架是基于Python环境实现的，所以需要先安装Python环境，由于担心将电脑其他环境破坏，使用conda创建Python环境。conda的安装步骤如下。

anaconda比较臃肿，直接安装[miniconda](https://docs.anaconda.com/free/miniconda/index.html)就行，官网下载太慢的话可以去[清华镜像源Miniconda镜像](https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/)选择指定版本的Conda镜像，下载`Miniconda3-latest-Windows-x86_64.exe`就行了。

下载过程中就选择一下安装目录，记得在选择使用对象的时候选用他推荐的，就是仅个人使用那个选项，不要选择全局安装，那样每次都要用管理员权限。这个部分往后的教程，都是写的选择个人使用的方案。注意安装路径最好不要在C盘。

安装完成后，打开`Miniconda自带的终端软件`，可以使用以下命令创建新环境

```BAHS
conda create -n mkdocs python=3.12
```

可以通过以下指令查看安装的环境

```BASH
conda env list
```

创建完成后使用以下命令跳转到该环境

```bash
conda activate mkdocs
```

要退出的话

```BASH
conda deactivate 
```

删除目标环境

```bash
conda env remove -n mkdocs
```

如果上述流程下载太慢，可以添加清华源，国外源下载包可能比较慢，所以我们查看清华源的文档，进行换源。[官方帮助文档](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda)

2024_12_05:现在文档的步骤是

```BASH
conda config --set show_channel_urls yes
```

就会在用户目录下如`C:\Users\<YourUserName>\.condarc`生成一个`.condarc`，然后将该文件打开，加入以下内容

```BASH
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

即可添加 Anaconda Python 免费仓库。

一般安装python库的话，要么使用pip安装，要么使用conda安装，最好不要同时使用，相互间的依赖文件可能会冲突。但是有的时候，缺少一些库文件的时候，可以用pip把对应的包删除掉，再使用conda安装，可能会安装好。

## pip换源

在刚建立好的conda的环境下，使用以下指令进行pip的换源。刚刚换的源是Conda的源，但是我们很多时候可能使用的是pip来进行安装下载。所以需要将pip的源换成国内源。

```BASH
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

## MkDocs创建流程

上面安装好了conda，而且我们建立了一个新环境mkdocs，后面的功能实现全在mkdocs虚拟环境上，记得需要用`Miniconda自带的终端软件`，不然没有conda环境。

安装mkdocs，只需要在控制台运行如下命令即可：

```BASH
pip install mkdocs-material
```

查看mkdocs是否安装成功，只需要运行如下命令：

```BASH
mkdocs --version
```

创建一个站点

```BASH
mkdocs new [dir_name]
```

然后进入我们创建的目录`dir_name`，运行如下命令，即可在本地访问站点，一般是`http://127.0.0.1:8000/`

```BASH
mkdocs serve
```

这样的只是建立了一个基本站点，后续就是配置MkDocs的主题。

## Material主题设置

打开刚刚生成的配置文件。

在上面加上主题配置，如下

```YAML
site_name: My Docs
theme:
  name: material
  language: zh
```

再返回实时渲染的页面，等待刷新，就会变成material格式的最新页面了。

其他的细节配置，可以参考他的网页配置教程，一个个的设置，他的网页上面还有一些比较细节的配置。

我这里放上我的配置，可以参考，也可以自己尝试修改内容。我的配置文件还需要安装几个插件。

```BASH
pip install mkdocs-glightbox   #一个图片放大的插件
```

下面是我的完整配置文件，记得文件的保存格式要是UTF-8，新建的MD文件一样要UTF-8：

```YML
site_name: LQH's Wiki  #网站名
site_url: https://github.com/snqx-lqh/wiki 
repo_url: https://github.com/snqx-lqh/wiki
repo_name: snqx-lqh/wiki
edit_uri: "" # edit/master
site_description: 一个自己的知识库网站，包含内容主要是嵌入式和图像处理相关，也有部分杂项内容。


theme:
  name: material
  language: zh
  #logo: assets/avatar.jpg      #logo 这里是放链接和下面的有一个就行
  favicon: assets/notebook.png  #网站图标 assets/avatar.jpg  
  icon:                         #图标
    logo: material/notebook     #logo 这里是放已有的可以在后面这个链接搜索 https://squidfunk.github.io/mkdocs-material/reference/icons-emojis/#search
    previous: fontawesome/solid/angle-left
    next: fontawesome/solid/angle-right
    repo: fontawesome/brands/git-alt  
    # edit: material/pencil
    # view: material/eye
  font:
    code: Roboto Mono  #https://fonts.google.com/ 配置代码字体 这个网站找喜欢的字体
    text: Roboto 
  features:
    # - navigation.instant            #预加载
    # - navigation.instant.prefetch   #鼠标悬停链接就开始获取页面 
    # - navigation.instant.progress   #进度指示器
    # - navigation.instant.preview    #数据预览
    - navigation.tracking             #启用锚点跟踪后，地址栏中的URL会自动更新为目录中突出显示的当前活动锚点。
    - navigation.tabs                 #就是最上面那一行导航
    - navigation.tabs.sticky          #向下滚动时导航可见
    # - navigation.sections           #章节功能，类似自动展开左边的列表
    - navigation.expand               #默认展开所有列表  没成功，不知道为啥
    - navigation.path                 #在文章最上面显示路径 没成功，不知道为啥
    - navigation.prune                #只有可见的会被渲染，减少构建大小 
    - navigation.indexes              #indexmd作为概览，点击文件合集就会显示
    - navigation.top                  #返回顶部的按钮
    # - navigation.footer             #设置底部,上一页和下一页
    - toc.follow                      #侧边栏可以自由滑动
    # - toc.integrate                 #把右边侧边栏隐藏
    
    - search.suggest                  # 搜索建议
    - search.highlight
    - search.share

    - announce.dismiss                #已读公告

    # - content.action.edit           #可以编辑
    # - content.action.view           #可以查看

    - content.code.copy

  
  # 主题背景切换的相关设置
  palette:
    # Palette toggle for automatic mode  自动模式调色板切换
    - media: "(prefers-color-scheme)"
      toggle:
        icon: material/brightness-auto
        name: Switch to light mode
    # Palette toggle for light mode  灯光模式的调色板切换
    - media: "(prefers-color-scheme: light)"
      scheme: default 
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    # Palette toggle for dark mode  深色模式的调色板切换
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      toggle:
        icon: material/brightness-4
        name: Switch to system preference
 
extra:
  annotate:
    json: [.s2]
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/snqx-lqh
    - icon: fontawesome/brands/bilibili
      link: https://space.bilibili.com/336653490?spm_id_from=333.1007.0.0

markdown_extensions:
  - abbr
  - admonition
  - attr_list
  - def_list
  - footnotes
  - md_in_html
  - toc:
      permalink: true
  - pymdownx.arithmatex:
      generic: true
  - pymdownx.betterem:
      smart_enable: all
  - pymdownx.caret
  - pymdownx.details
  - pymdownx.emoji:
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
      emoji_index: !!python/name:material.extensions.emoji.twemoji
  - pymdownx.highlight:
      anchor_linenums: true
      line_spans: __span
      pygments_lang_class: true
  - pymdownx.inlinehilite
  - pymdownx.keys
  - pymdownx.magiclink:
      normalize_issue_symbols: true
      repo_url_shorthand: true
      user: squidfunk
      repo: mkdocs-material
  - pymdownx.mark
  - pymdownx.smartsymbols
  - pymdownx.snippets:
      auto_append:
        - includes/mkdocs.md
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
  - pymdownx.tabbed:
      alternate_style: true
      combine_header_slug: true
      slugify: !!python/object/apply:pymdownx.slugs.slugify
        kwds:
          case: lower
  - pymdownx.tasklist:
      custom_checkbox: true
  - pymdownx.tilde
 

copyright: Copyright &copy; 2020 - 2025 snqx-lqh

plugins:
  - glightbox # 图片放大
  - search: # 搜索(选择支持中文、英文)
      separator: '[\u200b\u3000\-、。，．？！；\s\-,:!=\[\]()"/]+|(?!\b)(?=[A-Z][a-z])|\.(?!\d)|&[lg]t;'
      # jieba_dict: jieba_dict/dict.txt.big
      # jieba_dict_user: jieba_dict/user_dict.txt
      lang: 
        # - zh
        - ja
        - en
```

可以在文件夹中创建一些内容，尝试
## github pages 部署

部署的话，我们使用githubpages实现，首先先建立一个github的仓库，我这里命名，mkdocs-test
![](image/Pasted%20image%2020250221110008.png)
创建完成后，使用git工具将刚刚的mkdocs内容传上去，只上传`site`文件夹中的东西，前提是你的电脑有安装git工具，这个不细讲。

```BASH
cd  site
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/snqx-lqh/mkdocs-test.git
git push -u origin master
```

上传后，刷新上传的页面，然后打开gitpages的功能，点击save就会开始自动部署，等待一会就会部署成功，然后后面就是有更新再上传即可。

![](image/Pasted%20image%2020250221111559.png)

过一段时间就会出现部署成功。

![](image/Pasted%20image%2020250221112036.png)

## Cloudflare 部署

国内访问github比较慢，可以使用[Cloudflare](https://dash.cloudflare.com/)创建一个镜像站，先注册账号登录进去。然后选择pages部署。

![](image/Pasted%20image%2020250221112732.png)
![](image/Pasted%20image%2020250221112757.png)

选择刚刚建立的存储库，然后直接下一步保存并部署。

![](image/Pasted%20image%2020250221112831.png)


部署成功会出现以下内容，第一次需要等待一段时间才能成功。

![](image/Pasted%20image%2020250221113004.png)
## 自定义设计

主页如果想实现我那样的内容，需完成如下配置。

首先是添加一个css文件夹，里面新建一个文件，`index.css`。

![](image/Pasted%20image%2020250221130310.png)

然后将index.md和index.css里面的文件分别改成如下设计，其中md文件的每个小块内容可以自己修改。

```Markdown
---
title: 主页
hide:
  - navigation
  - toc

---

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<link rel="stylesheet" href="stylesheets/index.css">

<script src="https://cdnjs.cloudflare.com/ajax/libs/prefixfree/1.0.7/prefixfree.min.js"></script>

<div class="hero">
    <h1>LQH's Wiki</h1>
    <p class="subtitle">
        个人知识库 | 技术分享 | 学习笔记
    </p>
</div>

<div class="introduction">
    <p>
        这是我的个人知识库，基于 <a href="https://www.mkdocs.org/">MkDocs</a> 搭建，
        使用 <a href="https://squidfunk.github.io/mkdocs-material">Material</a> 主题
    </p>
    <p>
        在这里，我收录了一些长篇且具有关联性的技术笔记，欢迎随意浏览！
    </p>
</div>

<!-- 开源项目 -->
<div class="open-source-container">
    <h2>🚀 个人开源项目</h2>
    <div class="project-grid">
        <!-- 示例项目1 begin-->
        <div class="project-card">
            <div class="project-header">
                <i class="fas fa-code icon"></i>
                <a href="https://github.com/snqx-lqh/Stm32BalanceCar" class="project-title" target="_blank">Stm32BalanceCar</a>
            </div>
            <p class="project-description">基于STM32C8T6的平衡车代码设计，使用CubeMX和FreeRTOS的一个项目</p>
        </div>
        <!-- 示例项目1 end-->

        <!-- 示例项目2 begin-->
        <div class="project-card">
            <div class="project-header">
                <i class="fas fa-code icon"></i>
                <a href="https://github.com/snqx-lqh/RaspberryPiSmartHome" class="project-title" target="_blank">RaspberryPiSmartHome</a>
            </div>
            <p class="project-description">树莓派智能家居项目，学习树莓派的wiringpi用C语言开发，并且组合成一个智能家居项目，使用的是树莓派3B+</p>
        </div>
        <!-- 示例项目2 end-->

        <!-- 示例项目3 begin-->
        <div class="project-card">
            <div class="project-header">
                <i class="fas fa-code icon"></i>
                <a href="https://github.com/snqx-lqh/Stm32RemoteControl" class="project-title" target="_blank">Stm32RemoteControl</a>
            </div>
            <p class="project-description">基于STM32F103cbt6的遥控器，使用FreeRTOS实时操作系统，通信使用NRF24L01</p>
        </div>
        <!-- 示例项目3 end-->

        <!-- 示例项目4 begin-->
        <div class="project-card">
            <div class="project-header">
                <i class="fas fa-code icon"></i>
                <a href="https://github.com/snqx-lqh/ProteusAnd89C51" class="project-title" target="_blank">ProteusAnd89C51</a>
            </div>
            <p class="project-description">使用Proteus8.9仿真51单片机的一些实例，包含数码管、LCD1602、步进电机、矩阵键盘、DS1302、超声波测距、DS18B20、蜂鸣器、EEPROM等</p>
        </div>
        <!-- 示例项目4 end-->


        <div class="project-card">
            <div class="project-header">
                <i class="fas fa-code icon"></i>
                <a href="https://github.com/snqx-lqh/SmartCarFourWheel" class="project-title" target="_blank">SmartCarFourWheel</a>
            </div>
            <p class="project-description">基于STM32F103RCT6的电机控制板，使用的是STM32的标准库，使用了FreeRTOS框架</p>
        </div>

        <div class="project-card">
            <div class="project-header">
                <i class="fas fa-code icon"></i>
                <a href="https://github.com/snqx-lqh/RaspberryPiLearningNotes" class="project-title" target="_blank">RaspberryPiLearningNotes</a>
            </div>
            <p class="project-description">树莓派学习记录库，包含学习时的树莓派操作，GPIO控制、IIC、SPI、UART，包含wiringPi、BCM2835、Python多种方式的使用</p>
        </div>
         
    </div>
</div>

<div class="contact-container">
    <h2>📬 联系方式</h2>
    <div class="contact-grid">
        <!-- 代码托管 -->
        <div class="contact-card">
            <h3><i class="fas fa-code-branch"></i> 我的代码托管</h3>
            <div class="link-grid">
                <a href="https://github.com/snqx-lqh" class="link-item" target="_blank" >
                    <i class="fab fa-github"></i>
                    <span>GitHub</span>
                </a>
                <a href="https://gitee.com/snqx-lqh" class="link-item" target="_blank" >
                    <i class="fab fa-git-alt"></i>
                    <span>Gitee</span>
                </a>
            </div>
        </div>

        <!-- 社交媒体 -->
        <div class="contact-card">
            <h3><i class="fas fa-share-alt"></i> 社交媒体</h3>
            <div class="link-grid">
                <a href="https://space.bilibili.com/336653490" class="link-item" target="_blank" >
                    <i class="fab fa-bilibili"></i>
                    <span>Bilibili</span>
                </a>
                <a href="https://blog.csdn.net/wan1234512" class="link-item" target="_blank" >
                    <i class="fas fa-blog"></i>
                    <span>CSDN</span>
                </a>
            </div>
        </div>

        <!-- 联系我 -->
        <div class="contact-card">
            <h3><i class="fas fa-envelope"></i> 联系我</h3>
            <div class="link-grid">
                <a href="mailto:liqinghuaxx@163.com" class="link-item" target="_blank" >
                    <i class="fas fa-envelope"></i>
                    <span>Email:liqinghuaxx@163.com</span>
                </a>
                <a href="https://github.com/snqx-lqh/wiki" class="link-item" target="_blank" >
                    <i class="fas fa-book"></i>
                    <span>Wiki项目</span>
                </a>
            </div>
        </div>
    </div>
</div>
```

css文件内容如下

```css
/* ========== Hero区域 ========== */
.hero {
    padding: 2rem 0;
    text-align: center;
    /* 新增居中 */
    position: relative;
    overflow: hidden;
    margin: 2rem 0;
    background:
        linear-gradient(135deg, #e3f2fd 0%, #bbdefb 100%),
        repeating-linear-gradient(45deg,
            rgba(255, 255, 255, 0.1) 0px,
            rgba(255, 255, 255, 0.1) 2px,
            transparent 2px,
            transparent 4px);
    /* 网格背景 */
    border-radius: 16px;
    box-shadow: 0 8px 24px rgba(33, 150, 243, 0.15);
}

.hero::after {
    content: "";
    position: absolute;
    top: 0;
    left: -50%;
    width: 200%;
    height: 100%;
    background: linear-gradient(90deg,
            transparent 25%,
            rgba(255, 255, 255, 0.2) 50%,
            transparent 75%);
    animation: shine 3s infinite;
}

@keyframes shine {
    0% {
        transform: translateX(-50%);
    }

    100% {
        transform: translateX(50%);
    }
}

.hero h1 {
    position: relative;
    z-index: 1;
    font-size: 3.5rem;
    color: #0d47a1;
    /* 深蓝色 */
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 1.5rem;
}

.subtitle {
    font-size: 1.5rem;
    color: #1976d2;
    /* 中蓝色 */
    letter-spacing: 0.5px;
    opacity: 0.9;
}

/* ========== 头部 响应式设计 begin========== */
@media (max-width: 768px) {
    .hero h1 {
        font-size: 2.5rem;
    }

    .subtitle {
        font-size: 1.2rem;
    }

    .contact-card h3 {
        font-size: 1.1rem;
    }

    .link-item {
        padding: 0.8rem;
    }
}
/* ========== 头部 响应式设计 end========== */



/* ========== 更新后的联系方式区域 ========== */
:root {
    --primary-blue: #2196F3;
    --hover-blue: #1976D2;
    --light-blue: #E3F2FD;
}

.contact-container {
    margin: 3rem 0;
}

.contact-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 2rem;
    margin-top: 1.5rem;
}

.contact-card {
    background: #ffffff;
    border: 1px solid #e0e0e0;
    /* 新增边框 */
    border-radius: 10px;
    padding: 1.5rem;
    box-shadow: 0 4px 12px rgba(33, 150, 243, 0.1);
    transition: transform 0.2s, box-shadow 0.2s;
}

.contact-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 16px rgba(33, 150, 243, 0.2);
}

.contact-card h3 {
    margin: 0 0 1rem 0;
    color: var(--primary-blue);
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: 1.3rem;
}

.link-grid {
    display: grid;
    gap: 1rem;
}

.link-item {
    display: flex;
    align-items: center;
    padding: 1rem;
    background: var(--light-blue);
    border-radius: 8px;
    color: #1E88E5;
    /* 链接默认颜色 */
    text-decoration: none;
    transition: all 0.2s;
    border: 1px solid transparent;
}

.link-item:hover {
    background: var(--primary-blue);
    color: white;
    border-color: var(--hover-blue);
}

.link-item i {
    font-size: 1.5rem;
    margin-right: 1rem;
    width: 30px;
    text-align: center;
    transition: color 0.2s;
}


/* ========== 原有保留样式 ========== */
.introduction {
    background: var(--md-code-bg-color);
    padding: 2rem;
    border-radius: 8px;
    margin: 2rem 0;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}


/* ========== 开源项目样式  begin ========== */
.open-source-container {
    margin: 3rem 0;
}

.open-source-container h2 {
    color: #0d47a1;
    border-bottom: 2px solid #e3f2fd;
    padding-bottom: 0.5rem;
    margin-bottom: 1.5rem;
}

/* ========== 卡片视觉优化 ========== */
.project-card {
    background: rgba(227, 242, 253, 0.3); /* 半透明浅蓝背景 */
    border: 1px solid rgba(33, 150, 243, 0.15);
    border-radius: 12px;
    padding: 1.5rem;
    transition: all 0.25s ease;
    backdrop-filter: blur(2px); /* 毛玻璃效果 */
}

.project-header {
    display: flex;
    align-items: center;
    gap: 0.8rem;
    margin-bottom: 1rem;
}

.icon {
    color: #2196F3; /* 主色图标 */
    font-size: 1.4rem;
    min-width: 28px; /* 固定图标宽度 */
}

.project-title {
    font-size: 1.2rem;
    font-weight: 600;
    color: #0d47a1;
    text-decoration: none;
    line-height: 1.3;
    transition: color 0.2s;
}

.project-title:hover {
    color: #1565c0;
    text-decoration: underline;
    text-underline-offset: 0.2em; /* 下划线间距 */
}

.project-description {
    color: #455a64; /* 深灰蓝色 */
    font-size: 0.95rem;
    line-height: 1.6;
    margin: 0;
    padding-left: 2.2rem; /* 与图标对齐 */
}

/* ========== 悬停动效增强 ========== */
.project-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 24px rgba(33, 150, 243, 0.12);
    background: rgba(227, 242, 253, 0.4);
}

/* ========== 响应式优化 ========== */
@media (max-width: 768px) {
    .project-card {
        padding: 1.2rem;
    }
    
    .project-title {
        font-size: 1.1rem;
    }
    
    .project-description {
        padding-left: 0;
        margin-top: 0.8rem;
    }
    
    .icon {
        min-width: 24px;
        font-size: 1.2rem;
    }
}
/* ========== 开源项目样式  end ========== */

/* ========== 开源项目卡片 响应式优化方案  begin========== */
.project-grid {
    display: grid;
    gap: 1.5rem;
    grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
}

@media (min-width: 1200px) {
    .project-grid {
        grid-template-columns: repeat(3, minmax(0, 1fr)); /* 严格3列 */
    }
}

@media (max-width: 1199px) and (min-width: 768px) {
    .project-grid {
        grid-template-columns: repeat(2, minmax(0, 1fr)); /* 中等屏幕2列 */
    }
    
    /* 奇数项目最后一项居中 */
    .project-card:nth-child(2n+1):last-child {
        grid-column: 1 / -1;
        max-width: 400px;
        margin: 0 auto;
    }
}

/* 移动端保持单列 */
@media (max-width: 767px) {
    .project-grid {
        grid-template-columns: 1fr;
    }
}
/* ========== 开源项目卡片 响应式优化方案  end========== */
```