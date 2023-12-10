---
title: "Eclipse_mcu"
date: 2023-07-13T22:02:22+08:00
type: "post"
---

## 创建最简单的 Led2 闪烁程序

- 打开 eclipse 软件

- 创建项目

	File --> New --> Project

	选择 C Project

	{{< figure src="/img/eclipse_create_project/selecte_project_type.png" title="选择创建一个 C Project" >}}

- 开发板型号选择

	由于我使用的开发板型号是 Nucleo-F401RE

	所以选择 'STM32F4xx C/C++ Project'

	{{< figure src="/img/eclipse_create_project/select_board_type.png" title="选择开发板型号" >}}

	并在 Project name 中输入项目名称

	然后点击 Next

- 目标开发版设置 

	{{< figure src="/img/eclipse_create_project/target_processing_setting.png" title="选择开发板设置" >}}

	然后点击 Next

- 项目目录结构设置

	{{< figure src="/img/eclipse_create_project/folder_set.png" title="选择开发板设置" >}}

	然后点击 Next

- 项目配置

	勾选上 Debug 和 Release

	然后点击 Next

- 工具链配置
	Toolchain path: 
	
	/[path-of-toolchain-install]/arm-gnu-toolchain-12.2.mpacbti-rel1-x86_64-arm-none-eabi/bin

	工具链下载链接

- 项目创建完成

	点击 finish


进入 include/led.h
修改宏定义
```
#define BLINK_PORT_NUMBER (0)
#define BLINK_PIN_NUMBER (5)
```
然后就可编译生成 hex 和 elf 文件
