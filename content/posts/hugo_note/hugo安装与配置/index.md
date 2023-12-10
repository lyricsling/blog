---
title: "hugo安装与配置"
date: 2023-04-16T22:46:30+08:00
type: "post"
showTableOfContents: true
---

# Hugo 安装
安装过程很简单，在 manjaro 自带的添加删除管理软件中下载即可。

## Hugo 生成站点
	Hugo new site [site_name]
	cd [site_name]
	
输入命令后，hugo 会为我们生成名为 site_name 的目录，目录下包含 hugo 所需的结构。

	
## Hugo 创建新文章

	hugo new posts/first_post.md
	
	
输入命令后， hugo 会生成名为 site_name/content/posts/ 的目录。 并在目录中生成 first_post.md 文件。即可打开编辑器对此文件进行编辑。

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