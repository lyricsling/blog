---
title: "TodoListApp"
date: 2024-02-10T09:45:20+08:00
draft: true
type: "post"
showTableOfContents: true
---

## Settings 控件
用于保存用户的一些相关设置。
```
Window {
	id: window
	
	width: 800
	height: 600
	
	Settings {
		property alias x: window.x
		property alias y: window.y
		property alias width: window.width
		property alias height: window.height
	}
}
```