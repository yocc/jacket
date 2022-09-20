# Pointer & referer & new & make

---

[TOC]



## Overview

implicit



除了切片(slice)、集合(map)、通道(channel)、func(函数)和接口(interface)之外, 其它的都是值传递, 结构体就是值传递

new(), 返回地址, 这个地址指向的不是值本身, 即值的首地址, 而是 这个值的 make 3字段结构体的地址, make结构体的地址
make(), 返回实际实质值的首地址, 只能为 slice, map, channel 分配内存

new()和make()返回的都是地址

`&` 取地址
`*` 解引用 或者 取值, 取地址指向的值

![image-20220724164027907](/Users/yocc/Library/Application Support/typora-user-images/image-20220724164027907.png)



## FAQ and troubleshooting















## See Also

https://go.dev/tour/methods/15











