---
title: "函数模板"
date: 2023-12-10T16:40:10+08:00
draft: false
type: "post"
showTableOfContents: true
tags: ["cpp"]
---

## 从一个简单的函数模板开始入手

```cpp
tempalte <typename T>
T max(T a, T b)
{
	return b < a ? a : b;
}
```
函数模板定义了同一家族的一系列算法。

什么是同家族的算法。这个算法可以被不同的类型调用，因为函数的型参的类型是被参数化的（parameterized）。

如上例的参数 a 和 b 是 template arguments。它们的参数类型是 template parameter T。它的参数类型是任意的。

template parameter 是在 template<typename T> 这一语句中申明的。

typename 定义了一个 T,他的类型为 type parameter。代表了它是任意类型，可以用任意类型来对他进行取代。

```cpp
max(3.3, 4.5);
```
把 template parameters 替换为固定的类型的过程称为实例化（instantiation）

### Two-Phase Translation
函数参数是如何被实例化的呢。

它分为两个步骤：

1. 发生在模板函数定义时，实例化之前的这个阶段。它检测除 template parameters 之外的其他语法错误。

2. 发生在实例化阶段，把函数参数替换后，再次检测语法是否正确。

#### Compiling and Linking
