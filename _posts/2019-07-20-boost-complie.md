---
layout: post
title:  "Boost 编译相关"
date:   2019-07-20 13:42:38 +0800
categories: boost
---

## Boost 的部分编译选项

### 用法

```shell
b2 [options] [properties] [install\|stage]
```

### 选项

| 选项 | 可选的值 | 解释 |
| :---: | :---: | :---: |
| install |  | 将头文件和编译后的库安装到指定的位置 |
| --prefix | *指定目录 <**PREFIX**>* | 平台无关的文件的安装位置 |
|  |  | （Windows 下默认 C:\Boost，其它系统默认 /usr/local） |
| --exec-prefix | *指定目录 <**EPREFIX**>* | 平台相关文件的安装位置 |
|  |  | 默认 <**PREFIX**> |
| --libdir | *指定目录 <**LIBDIR**>* | 库文件的安装位置 |
|  |  | 默认 <**EPREFIX**> |
| --includedir | *指定目录 <**HDRDIR**>* | 头文件的安装位置 |
|  |  | 默认 <**PREFIX**>/include |
| --cmakedir | *指定目录 <**CMAKEDIR**>* | CMake 配置文件的安装位置 |
|  |  | 默认 <**LIBDIR**>/cmake |
| --no-cmake-config |  | 不安装 CMake 的配置文件 |
| stage |  | 构建但仅安装编译好的库文件到指定位置 |
| --stagedir | *指定目录 <**STAGEDIR**>* | 库文件的安装位置 |
|  |  | 默认 ./stage |

### 其它选项

| 选项 | 可选的值 | 解释 |
| :---: | :---: | :---: |
| --build-type | minimal\|complete |  |
| --build-dir | <**DIR**> | 将构建的中间结果放到指定目录 |
| --show-libraries |  | 显示 Boost 中需要构建的库 |
| --layout | versioned\|tagged\|system | 指定生成的库文件名的格式 |
|  |  | Windows 默认 versioned，Unix 下默认 system |
| --buildid | <**ID**> | 将指定的 **ID** 添加到库文件名中，默认没有 |
| --python-buildid | <**ID**> | 为依赖 Python 的库添加 **ID**，这还会添加 --buildid 指定的 **ID** |
| --help |  | 显示帮助信息 |
| --with-*模块名* | atomic\|chrono\|... | 指定需要编译的模块 |
| --without-*模块名* | atomic\|chrono\|... | 需要排除的模块(和 --with- 互斥) |

### Properties:

| 选项 | 可选的值 | 解释 |
| :---: | :---: | :---: |
| --toolset | msvc-14.2\|... | 用于构建的工具集 |
| variant | debug\|release |  |
| debug |  |  |
| release |  |  |
| link | static\|shared |  |
| runtime-link | static\|shared |  |
| threading | single\|multi | 在单线程还是多线程的环境下使用(multi 也不保证线程安全) |
| address-model | x32\|64 | 生成多少位的库 |

### 常规命令行的用法

```shell
b2 [options] [properties] [targets]
# Options, properties and targets 可以任意顺序指定
```

### Important Options:

| 选项 |解释 |
| :---: | :---: |
| --clean | 清除构建的目标 |
| -a | 重新构建所有 |
| -n | 输出要执行的命令但执行它们 |
| -d+2 | 执行命令时显示命令 |
| -d0 | 禁止显示所有信息性消息 |
| -q | 在第一次出现错误时停止 |
| --reconfigure | 重新运行所有配置检查 |
| --debug-configuration | 诊断配置 |
| --debug-building | 报告使用哪些 properties 构建的目标 |
| --debug-generator | Diagnose generator search/execution |

### 更多的帮助文档

以下选项可用于获取其他文档：

* --help-options 打印更少用的命令行选项  
* --help-internal Boost.Build 实施细节  
* --help-doc-options Implementation details doc formatting.  

## 其它

### 一些标志在 windows 下输出的名字格式

| 配置 | 影响 |
| :--- | :--- |
| link=static | 生成的库文件名称以 "lib" 开头 |
| link=shared | 生成的库文件名称无 "lib" 开头 |
| threading=mult | 生成的库文件名称中包含 "-mt" |
| variant=release | 生成的库文件名称不包含 "-gd" |
| variant=debug | 生成的库文件名称包含 "-gd" |
| runtime-link=static | 生成的库文件名称包含 "-s" |
| runtime-link=shared | 生成的库文件名称不包含 "-s" |


参考资料

1. [KAME. boost编译随笔](https://www.cnblogs.com/sanghg/p/5475044.html).
2. [雅各宾. BOOST编译链接选项link 和 runtime-link，搭配shared 和 static](https://my.oschina.net/jacobin/blog/171441).
3. [c++ - 在編譯boost時，`threading=multi` 究竟做了什麼？](http://hant.ask.helplib.com/others/post_1526219).
