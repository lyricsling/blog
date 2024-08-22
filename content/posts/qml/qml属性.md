---
title: "Qml属性"
date: 2024-02-08T09:52:39+08:00
type: "post"
showTableOfContents: true
---

# qml 属性
## id 属性
每一个 qml object 都有一个 id 属性。
id 属性可以被赋值，用于 qml object 的唯一标识符，以便被其他 obejct 引用。
id 以小写字母开头。

Text 使用 TextInput的 id 去访问其 text。
``` qml
Column {
	width: 200; height: 200
	TextInput { id: myTextInput; text: "Hello World" }
	Text { text: myTextInput.text }
}
```

## Attributes 的属性
在 c++ 类中，可以通过 Q_PROPERTY 注册属性至 qml 系统。
同样，也可以在 qml 文档中定义属性。

他的语法如下：
```
	[default] [required] [readonly] property <propertyType> <propertyName>
```
property name 只能以小写字母开头
<u>default property</u>
<u>required property</u>
<u>readonly property</u>
 
property 中隐藏一个信号处理机制

on<PropertyName>Changed
```
Rectangle {
	property color previousColor
	property color nextColor
	onNextColorChanged: console.log("The next color will be: " + nextColor.toString())
``` imperative

### property 的类型
除了枚举类型，都支持
<u>QML Value Types</u>

var 支持任何类型
```
property var someNumber: 1.5
property var someString: "abc"
property var someBool: true
property var someList: [1, 2, "three", "four"]
property var someObject: Rectangle { width: 100; height: 100; color: "red"}
```
### 对 property 赋值
- 初始化赋值
	```
	Rectangle {
		color: "red"
		property color nextColor: "blue" // combined property declaration and initialization
	}
	```
- 赋值语句
	```
	Rectangle {
		id: rect
		Component.onCompleted: {
			rect.color = "red"
		}
	}
	```
	
### static values 和 binding Expression Values
```
	Rectangle {
		// both of these are static value assignments on initialization
		width: 400
		height: 200
		
		Rectangle {
			// both of these are binding expression value assignments on initialization
			width: parent.width / 2
			height: parent.height
		}
	}
```
<u>Property Binding</u>

### 特殊的 property type
语法
list
```
	[<item 1>, <item 2>, ... ]
```

使用
```
	Item {
	 states: [
	 	State { name: "loading" },
	 	State { name: "running" },
	 	State { name: "stopped" }
	 ]
	}
```
初始化
```
	Rectangle {
	property list<Rectangle> childRects: [
		Rectangle { color: "red" },
		Rectangle { color: "blue" }
	]
	}
```

### group property
既可以使用 dot notation，也可以使用 group notation
```
Text {
	font.pixelSize: 12
	font.b: true
}

Text {
	font: { pixelSize: 12; b: true}
}
```

### alias

### alias 使用的注意事项

```
// MyItem.qml
Item {
	property alias inner : innerItem
	
	Item {
		id: innerItem
		property int extraProperty
	}
}

// main.qml
MyItem {
	inner.extraProperty: 5 // fails
}
```

## Signal Attributes
<u>signal handler</u>

on<Signal>

```
Item {
	width: 100; height: 100
	
	MouseArea {
		anchors.fill: parent
		onClicked: {
			console.log("Click")
		}
	}
}
```
### 定义信号
Q_SIGNAL 定义的信号可以被 QML 系统识别
或在 QML 文档中直接定义
```
signal <signalName>
```
三种信号的申明形式均可：
```
Item {
	signal clicked
	signal hovered()
	signal actionPerformed(action: string, actionResult: int)
}
```

## 信号处理
定义了信号后，会自动生成对应的空的 signal handler

```
// SquareButton.qml
Rectangle {
	id: root
	
	signal actived(xPosition: real, yPosition: real)
	signal deactivated
	
	property int side: 100
	width: side; height: side
	
	MouseArea {
		anchors.fill: parent
		OnReleased: root.deactivated()
		OnPressed: (mouse)=> root.actived(mouse.x, mouse.y)
	}
}

//main.qml
SquareButton {
	onDeactivated: console.log("Deactivated!")
	onActived: (xPosition, yPosition)=> console.log("Active at " + xPosition + "," + yPosition)
}
```
### Property Change Signal Handlers
on<Property>Changed
```
TextInput {
	text: "Change this!"
	onTextChanged: console.log("Text has changed to:", text)
}
```

## 定义方法
在 c++ 中定义 Q_INVOKABLE 或者 Q_SLOT
或者在 qml语法中定义如下：
```
function <functionName>([<parameterName>[: <parameterType>][, ...]]) [: <returnType>] { <body> }
```


# QML Document
QML Document 是一串符合 qml 语法的字符串，他定义了 QML 的 object type

会被创建为 Component 或 QQmlComponent

.qml 或  .ui.qml

## structure of a qml document
其内容由 imports section 和 declaration section 组成

define object types through qml documents

resource loading and network transparency

scope and naming resolution 

## define an object type with q qml file
file named as <TypeName>.qml is the desired name of the type
- must be comprised of alphanumeric characters or underscores.
- must be with an uppercase letter.

qml engine 会自动搜索 current directory

## inline components
components 只在当前文件中可以使用

```
//Images.qml
Item {
	component LabeledImage : Column {
		property alias source: image.source
		property alias caption: text.text
		
		Image {
			id: image
			width: 50
			height: t0
		}
		Text {
			id: text
			font.bold: true
		}
	}
	Row {
		LabeledImage {
			id: before
			source: "before.png"
			caption: "Before"
		}
		LabeledImage {
			id: after
			source: "after.png"
			caption: "After"
		}
	}
	property LabeledImage selectedImage: before
}

// main.qml
Rectangle {
	property alias caption: image.caption
	property alias source: image.source
	border.width: 2
	border.color: "black"
	Images.LabeledImage {//需要使用前缀
		id: image
	}
}
```