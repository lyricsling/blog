---
title: "无法加载svg图片"
date: 2024-09-30T13:48:38+08:00
type: "post"
showTableOfContents: true
---
在 qt 开发过程中，遇到了一个无法加载svg图片资源文件的问题。

图片以及添加进 qrc 文件中，并且使用时路径也正确。但是任然无法加载图片。

最后发现有以下几种原因可能导致上述情况：

- CMakeLists 文件中是否包含以下宏定义
	```
	set(CMAKE_AUTOUIC ON)
	set(CMAKE_AUTOMOC ON)
	set(CMAKE_AUTORCC ON)
	```
- 要包含 Qt 的 svg 库
	```
	find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Svg)
	target_link_libraries(qtoobar_test PRIVATE Qt${QT_VERSION_MAJOR}::Widgets Qt${QT_VERSION_MAJOR}::Svg)
	```

