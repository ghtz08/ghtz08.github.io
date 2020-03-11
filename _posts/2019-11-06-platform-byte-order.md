---
layout: post
title:  "判断不同平台的字节序"
date:   2019-11-06 15:55:00 +0800
categories: [cpu, platform]
---

## 判断不同的平台

```cpp
// 代码来自 SDL2 2.0.10 的 SDL_endian.h 文件
#define CPU_LIL_ENDIAN 1234
#define CPU_BIG_ENDIAN 4321

#ifdef __linux__
#   include <endian.h>
#   define CPU_BYTEORDER  __BYTE_ORDER
#else /* __linux__ */
#   if defined(__hppa__) || \
        defined(__m68k__) || defined(mc68000) || defined(_M_M68K) || \
        (defined(__MIPS__) && defined(__MISPEB__)) || \
        defined(__ppc__) || defined(__POWERPC__) || defined(_M_PPC) || \
        defined(__sparc__)
#       define CPU_BYTEORDER   CPU_BIG_ENDIAN
#   else
#       define CPU_BYTEORDER   CPU_LIL_ENDIAN
#   endif
#endif /* __linux__ */
```

> 可能无法判断 `pandora` 系统和 `wiz` 系统  
> 之所以这么说，因为在 `SDL_config_pandora.h` 和 `SDL_config_wiz.h` 中也定义了 `BYTEORDER` 为 `LIL_ENDIAN`

## 转换字节序

```cpp
// since c++ 11

template <typename T>
struct MultibyteConcepts
{
    static constexpr bool value =
        !std::numeric_limits<T>::is_signed &&
        1 < sizeof(T) &&
        1;
};

//template <typename T>
//inline constexpr auto MultibyteConceptsValue = MultibyteConcepts<T>::value;

template <typename T>
constexpr auto changeEndian
(
    T const num
) -> typename std::enable_if<MultibyteConcepts<T>::value, T>::type
{
    constexpr auto size = sizeof(T);
    union
    {
        T num;
        uint8_t bytes[size];
    }temp;
    temp.num = num;
    for (auto i = 0u; i != size / 2; ++i)
    {
        auto const t = temp.bytes[i];
        temp.bytes[i] = temp.bytes[size - i - 1];
        temp.bytes[size - i - 1] = t;
    }
    return temp.num;
}
```

## 编译器内置的字节序转换函数

### MSVC

> _byteswap_ushort  // in <cstdlib>

### GCC

> __builtin_bswap16
