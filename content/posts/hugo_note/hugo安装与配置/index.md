---
title: "hugo安装与配置"
date: 2023-04-16T22:46:30+08:00
type: "post"
showTableOfContents: true
tags: ["博客配置"]
---

# Hugo 安装
安装过程很简单，在 manjaro 自带的添加删除管理软件中下载即可。

## Hugo 生成站点
	Hugo new site [site_name]
	cd [site_name]
	
输入命令后，hugo 会为我们生成名为 [site_name] 的目录，目录下包含 hugo 所需的结构。

	
## Hugo 创建新文章

	hugo new posts/first_post.md
	
	
输入命令后， hugo 会生成名为 [site_name]/content/posts/ 的目录。 并在目录中生成 first_post.md 文件。即可打开编辑器对此文件进行编辑。

注意：删除 draft 字段，非草稿文档才可以在网页中正常渲染。
	
## 启动 Hugo
创建好站点和新文章后，输入以下命令，便可启动 Hugo 静态网站
	
	hugo server
	
	或者
	
	hugo server -D
	
打开浏览器，在地址栏中输入：http://localhost:1313
	
便可进行网页预览

## Hugo 主题安装
Hugo 有许多别人创建好的各式各样的主题。

可以根据个人喜好进行选择下载。

以 gokarna 主题为例：
1. 进入站点根目录
2. 进入 themes 目录 
3. 下载主题
	git clone https://github.com/526avijitgupta/gokarna.git
4. 编辑站点根目录配置文件 config.toml
	加入
	```
	theme = "gokarna"
	```
5. head font 中 加入
	```
	type: "post"
	```
6. 根据 gokarna 的安装教程，配置 config.toml 文件。

7. 自此，主题便配置成功了

注意：再次预览网站，发现文章消失了，需要在根目录下 config.toml 配置文件中加入以下内容

	[params]
		showPostsOnHomePage = "recent"

表示，需要在主页中显示最近添加的博客

 -> front matter 是什么
 
## 图片配置
如果我们想在网页中加入本地图片，

有两种方式可以实现

- **方式一**

	此种方式可以把跟网站有关，不经常变动的图片放入此处。如网站的背景图片等。
	
	需要在 static 文件夹中放入图片文件

	可以自己组织目录结构，如把图片放在 static/img/new.jpg 下

	然后执行命令 hugo

	此时，就会在 public 目录中生成 img/new.jpg 文件结构

	markdown 文件中就可以加入图片了

	{{< figure src="/img/new.jpg" width="30%" >}}
-  **方式二**

	使用 hugo 提供的包管理方式（bundles），此种方式可以把文件和网页打包在一起，方便后期管理。
	
	把原本为 “hugo 安装与配置.md” 文件变为目录，并把文件的内容放置于 index.md 中，并把图片放入同级目录下。
	
	content 中目录结构如下：
	
	{{< figure src="imgs/struct.png" width="70%" >}}
	
	此时图片就可以被正常访问了。
	
### 改变图片的大小

``` cpp
\{\{< figure src="elephant.jpg" title="An elephant at sunset" width="80%" >}}
```

### 部署
我采用的方案是使用 github 的 page 和 action 功能进行自动部署。

#### 创建仓库
先在 github 上创建一个新仓库，用于保存我们的 hugo 内容。

注意：新建的仓库一定要是 public 的，github 对 private 仓库使用部署功能是需要收费的。

设置仓库的 page 属性，选择 source 为 Github actions。

{{< figure src="imgs/pageset.png" width="100%" >}}

在仓库设置界面中进行如下设置，设置 Workflow permissions 为 Read and write permissions。

{{< figure src="imgs/actionset.png" width="100%" >}}

#### 远程仓库与本地仓库连接
```bash
# 先进入 hugo 根目录
cd ~/blog

# 用 git 管理当前目录
git init
git checkout -b main

# 添加主题为submodule
git submodule add https://github.com/526avijitgupta/gokarna.git themes/gokarna 

# 把当前内容暂存并提交
git add .
git commit

# 添加远程仓库
git remote add origin 'git@github.com:yourusername/blog.git'

# 推送至远程仓库
git push origin main
```

#### 添加自动部署配置文件

在 hugo 根目录下创建文件 .github/Workflow/hugo.yaml

加入如下内容：
```
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.121.0
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v4
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"          
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v3
```

内容挺多，下面做一些简短的介绍。

第一个值得注意的代码段是：
```
on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main
```
它表示在向 main 分支 push 内容时触发部署操作。

接下来值得注意的就是 jobs 之后的内容：

```
step:
    - name: Install Hugo CLI
    ...
    - name: Upload artifact
      uses: actions/upload-pages-artifact@v2
```
step 下每一个 name 都是一个步骤。其中 uses 是使用 github 上别人已经写好的一些执行步骤。

最后，提交.github/Workflow/hugo.yaml 文件后，github即可进行自动部署了。

#### 提交
提交完成后，在 github 上会多一个 action 提示。黄色圆圈表示正在进行部署操作中。
{{< figure src="imgs/autoact.png" width="90%" >}}

他生成的 url 如下，点击 url 就可访问我们的网站了。
{{< figure src="imgs/url.png" width="90%" >}}

