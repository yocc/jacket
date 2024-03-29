# Consistent Hashing

---

[toc]



## Overview

```shell
1. 已知:
    3个节点, node0, node1, node2
    3000万个key
2. 求:
    在3个节点上均匀分散所有key

3. 简单Hash()算法:
    对key进行hash运算后取模，N是节点的数量, hash(key) % N
解:
    hash(key)%3=(0, 1, 2), 模只会三种情况, 0, 1, 2
优点: 简单
缺点: 节点变化N从3变2, 那么key在各个节点上的分布大部分都会发生变化

4. 一致性Hash算法:
    一致性哈希解决了简单哈希算法Hash()在分布式哈希表(Distributed Hash Table, DHT)中存在的动态伸缩等问题;
解:
    简单哈希算法是对N(固定节点数)取模, 而一致性哈希算法是对固定值取模这个固定值是2的32次方取模, 即, hash(key) % (2^32)
    一定会得到一个 0 ~ 2^32 之间的整数
    1. 先构建一个环, 0 ~ 2^32-1 个位点的环
    2. 将服务器标识哈希后对2^32取模, hash(服务器标识)%(2^32)
    3. 将key哈希后对2^32取模, hash(key标识)%(2^32), 事实上, key的哈希函数与服务器哈希算法的哈希函数是不同的.
    4. 将key落入服务器, key在环上位置沿顺时针找到最近的服务器位置
    5. 虚拟节点来解决负载不均衡的问题, 将每个物理服务器扩展为多个虚拟节点(物理节点是虚拟节点的集合), 虚拟节点标识=物理节点标识#虚拟节点序号, hash(虚拟节点标识)%(2^32)
       虚拟节点数量越大, 哈希环分配的就越均匀
       引入虚拟节点的同时也增加了新的问题, 要做虚拟节点和真实节点间的映射, 对象key->虚拟节点->实际节点之间的转换;
优点: 一致性hash在分布式系统中应该是实现负载均衡的首选算法, 客户端上实现.
缺点:
总结: MC服务器之间不通讯, 全靠客户端通过一致性hash路由来决定访问的服务器.


```



## See Also

