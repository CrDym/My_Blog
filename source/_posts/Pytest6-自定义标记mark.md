---
title: Pytest6-自定义标记mark
date: 2021-06-29  23:45
tags: Pytest
categories: Python学习

---

## 前言

在pytest中，我们可以使用mark进行用例的自定义标记，通过不同的标记实现不同的运行策略

比如我们可以标记哪些用例是生产环境执行的，哪些用例是测试环境执行的，在运行代码的时候指定对应的mark即可

<!-- more -->

## 实例说明

举个🌰

```python
# test_demo.py
import pytest

@pytest.mark.production
def test_production():
    print("生产环境测试用例")


@pytest.mark.dev
def test_dev1():
    print("测试环境测试用例1")


@pytest.mark.dev
def test_dev2():
    print("测试环境测试用例2")


def testnoMark():
    print("没有标记测试")
```

使用命令`pytest -s -m dev test_demo.py`执行

结果如下

![image-20210629224049942](https://img.rockche.cn//image-20210629224049942.png)

可以看到，只执行了两条标记了dev的用例

### 处理warnings信息

- 创建一个pytest.ini文件

- 然后在 pytest.ini 文件的 markers 中写入你的 mark 标记， 冒号 “:” 前面是标记名称，后面是 mark 标记的说明，可以是空字符串

- **注意：**pytest.ini需要和运行的测试用例同一个目录，或在根目录下作用于全局

  ![image-20210629224639064](https://img.rockche.cn//image-20210629224639064.png)

### 规范使用mark标记

添加了pytest.ini文件之后 pytest 便不会再告警，但是如果我们在运行用例的时候写错了 mark 名，会导致 pytest 找不到用例，所以我们需要在 pytest.ini 文件中添加参数 `“addopts = --strict-markers”`来严格规范 mark 标记的使用

![image-20210629232426057](https://img.rockche.cn//image-20210629232426057.png)

添加该参数后，当使用未注册的 mark 标记时，pytest会直接报错：“ `'xxx' not found in markers configuration option` ”，不执行测试任务

![image-20210629232320091](https://img.rockche.cn//image-20210629232320091.png)

### 执行标记以外的用例

> `pytest -s -m "not dev" test_demo.py`

结果如下

![image-20210629225046830](https://img.rockche.cn//image-20210629225046830.png)

### 执行多个自定义标记的用例

> `pytest -s -m "dev or production" test_demo.py`

结果如下

![image-20210629225222295](https://img.rockche.cn//image-20210629225222295.png)