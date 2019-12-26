---
layout: post
title:  "编译器相关"
date:   2019-07-14 10:40:05 +0800
categories: 编译器
---

## 不同编译器特有的宏

- GNU: `__GNUC__` 的值是 GCC 的版本号，类型为 `int`
- MinGW: `__MINGW32__`
- MSVC: `_MSC_VER` 的值是 cl.exe 的版本, 类型为 `int`

| MSVC 编译器版本 | _MSC_VER 值 |
| :---: | :---:|
| MS VC++ 5.0                   | 1100 |
| MS VC++ 6.0                   | 1200 |
| MS VC++ 7.0                   | 1300 |
| MS VC++ 7.1                   | 1310 |
| MS VC++ 8.0 (Visual C++ 2005) | 1400 |
| MS VC++ 9.0 (Visual C++ 2008) | 1500 |

- symbian: `__SYMBIAN32_`
  - 3rd MR 版及之前的 3rd 版本: `__SERIES60_30__`
  - 3rd FP1 版: `__SERIES60_31__`
  - 3rd FP2 版: `__SERIES60_32__`
  - 所有版本: `__SERIES60_3x__`

> gcc 和 clang 可以用 `clang -dM -E -x c /dev/null` 来打印输出预定义的宏

## GNU 下将 typeid 得到类型名转为具有可读性的

```c
#include <cxxabi.h>
char const * const realname = abi::__cxa_demangle(typeid(0).name(), 0, 0, 0);
free(realname)
```

## 显示函数签名

- MSVC: __FUNCSIG__, __FUNCDNAME__
- GCC: __PRETTY_FUNCTION__


参考资料

1. [编译器预定义的宏(可以用来区分使用的是哪种编译器) 详细??](https://zhidao.baidu.com/question/239754894534682724.html)
2. [Chapter 28. Demangling](https://gcc.gnu.org/onlinedocs/libstdc++/manual/ext_demangling.html)
