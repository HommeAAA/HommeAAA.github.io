---
title: LRU设计题
tags:
  - 设计题
category:
  - 算法
  - LeetCode
  - 设计题
date: 2020-05-25 16:02:42
---
# 1. LRU设计题
## 1. [146. LRU Cache](https://leetcode-cn.com/problems/lru-cache/)
运用你所掌握的数据结构，设计和实现一个  LRU (最近最少使用) 缓存机制。它应该支持以下操作： 获取数据 get 和 写入数据 put 。

获取数据 get(key) - 如果密钥 (key) 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。
写入数据 put(key, value) - 如果密钥已经存在，则变更其数据值；如果密钥不存在，则插入该组「密钥/数据值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

### 题解
利用LinkedList和HashMap, **注意remove需要Integer**
```java
class LRUCache {
    LinkedList<Integer> list;
    Map<Integer, Integer> map;
    int capacity;

    public LRUCache(int capacity) {
        list = new LinkedList<>();
        map = new HashMap<>();
        this.capacity = capacity;
    }
    
    public int get(int key) {
        if (map.containsKey(key)) {
            //如果是remove(int)移除的则是索引
            list.remove((Integer)key);
            list.addFirst(key);
            return map.get(key);
        }
        return -1;
    }
    
    public void put(int key, int value) {
        //先判断是否存在
        if (map.containsKey(key)) {
            list.remove((Integer)key);
            list.addFirst(key);
            map.put(key, value);
            return;
        }
        //不存在若到达上限，则移除map里面list末尾的元素
        if (list.size() == capacity) {
            map.remove(list.removeLast());
            map.put(key, value);
            list.addFirst(key);
        } else {
            map.put(key, value);
            list.addFirst(key);
        }
    }
}
```