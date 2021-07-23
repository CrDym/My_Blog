---
title: Python练习1
date: 2021-07-23  19:20
tags: Python
categories: Python练习题
---

## 题目

有四个数字：1、2、3、4，能组成多少个互不相同且无重复数字的三位数？各是多少？

## 分析

可填在百位、十位、个位的数字都是 1、2、3、4。组成所有的排列后再去掉不满足条件的排列

## 实例

```python
n = 0

for i in range(1, 5):
    for j in range(1, 5):
        for k in range(1, 5):
            if (i != k) and (i != j) and (j != k):
                print(i, j, k)
                n += 1

print(f"共计 {n} 个")
```

