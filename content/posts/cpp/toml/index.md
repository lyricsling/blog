---
title: "toml 使用"
date: 2024-1-3T16:48:10+08:00
draft: false
type: "post"
showTableOfContents: true
tags: ["cpp"]
---
## toml 的使用
toml 的使用非常的简单，直接包含头文件就可以使用 toml 提供的接口了。

我包含头文件的办法如下：

在项目根目录创建 include 文件夹，把 toml.hpp 移动至文件夹下，在 cmake 中加入以下语句
```cpp
target_include_directories(toml_test PUBLIC "${CMAKE_SOURCE_DIR}/include")
```

最小测试代码

```cpp
#include "toml.hpp"
#include <boost/type_index.hpp>
#include <iostream>

using namespace std::string_view_literals;

int main(int argc, char *argv[])
{
    const auto path = argc > 1 ? std::string_view{ argv[1] } : "example.toml"sv;

    toml::table tbl;
    try
    {
        // read directly from stdin
        if (path == "-"sv || path.empty())
            tbl = toml::parse(std::cin, "stdin"sv);

        // read from a file
        else
            tbl = toml::parse_file(path);
    }
    catch (const toml::parse_error& err)
    {
        std::cerr << err << "\n";
        return 1;
    }
}
```
