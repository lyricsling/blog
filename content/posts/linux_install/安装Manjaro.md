---
title: "安装Manjaro"
date: 2023-04-18T22:14:50+08:00
type: "post"
---

# manjaro 安装后的配置
## 更新 pacman 的镜像列表
sudo pacman-mirrors -i -c China -m rank

然后进行系统更新
```
sudo pacman -Syu
```

## pacman 安装一些基础包
``` bash
sudo pacman -S base base-devel linux linux-headers linux-firmware nvim gdb git
```
## cn库添加与yay安装
编辑文件 /etc/pacman.conf,在文件尾添加：

```
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

```
# 添加后输入命令,更新镜像库，此时会多出archlinuxcn模块
sudo pacman -Syu
```

导入 GPG Key

```
sudo pacman -S archlinuxcn-keyring
```
安装yay

```
sudo pacman -S yay
```

配置 yay
```
yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save
```

注意yay不需要root权限执行

## 安装输入法 fcitx
manjaro 安装时语言选项选择了中文，系统好像会自动安装输入法(待验证)

用 manjaro 自带的商店安装
fcitx5-configtool fcitx5-chinese-addons-git fcitx5-git fcitx5-gtk-git gcitx5-qt5-git

编辑文件 ~/.xprofile

输入内容：

``` bash
export QT_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```

然后注销，再次登录后打开 fcitx-configtool 工具

后及安装成功，便可进行输入法切换

## 安装Qt
download.qt.io

leecok_ms@outlook.com

QtCreator 编辑-设置-环境-启用高DPI模式

## 桌面环境设置
右键配置显示设置，由于使用的是4k屏，需要设置缩放比例为175%，看起来图标才不会太小



## 安装clion
```
yay -S clion

sudo pacman -S jre
```
## 向日葵安装
```
yay -S sunloginclient
sudo systemctl start runsunloginclient.service
sudo systemctl enable runsunloginclient.service
```

## 进入黑暗系

### firefox 配置

以前都是使用插件进行黑暗系风格配置，始终会有闪烁的情况。

今天发现 firefox 自带有颜色渲染功能

在 设置->颜色
  
管理颜色-> 去掉使用背景颜色

并在下拉选项卡中选择 **一律** 

然后就可以自由选择文本颜色和背景颜色了

### 联想电脑背景灯
原来我的笔记本电脑的背景灯可以自由选择开与不开

Fn + Space

### kde主题
Breeze + 微风深色

### Terminal
选择了商店中的主题： bl1nk

默认终端设置为 zsh

去掉 manjro命令提示符前缀
nvim /usr/share/zsh/manjaro-zsh-prompt 注释掉 source /usr/share/zsh-theme-powerlevel10k/powerlevel10k.zsh-theme


### zathura

nvim .config/zathura/zathurarc 加入

```
set recolor true
set recolor-darkcolor "#7D7D7D"
set recolor-lightcolor "#1f1f1f"

set window-title-basename "true"
set selection-clipboard "clipboard"
```

### word

Tools -> options -> LibreOffice -> Application Colors -> dark

### F12
F12 可以打开悬浮终端


