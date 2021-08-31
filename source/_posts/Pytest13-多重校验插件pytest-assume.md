---
title: Pytest13-多重校验插件pytest-assume
date: 2021-07-23  22:45
tags: Pytest
categories: Python学习
---

## 前言

在pytest中，我们可以使用python的assert进行断言，也可以同时在一个用例中进行多个断言，但存在一个问题就是当一个断言失败后，后面的断言将不再执行。那么如何解决这个问题呢，我们可以使用pytest-assume这个插件

## pytest-assume插件

### 插件安装

pip命令安装

```shell
pip install pytest-assume
```

### 使用assert进行多重断言

```python
def test1():
    assert 1+1 == 2
    assert 2+3 == 5
    assert 3+1 == 5
    assert 3+3 == 6
    assert 4+4 == 8
    print("测试结束")
```

执行结果如下：

![image-20210723165423002](https://img.rockche.cn//image-20210723165423002.png)

可以看到，在第4行代码的断言失败后，后面的断言都没有被执行，包括正常的代码

### 使用pytest.assume断言

```python
import pytest


def test1():
    pytest.assume(1+1 == 2)
    pytest.assume(2+3 == 5)
    pytest.assume(3+1 == 5)
    pytest.assume(3+3 == 6)
    pytest.assume(4+4 == 5)
    print("测试结束")
```

运行结果如下

![image-20210723170209979](https://img.rockche.cn//image-20210723170209979.png)

可以看到，在有断言失败后，后面的断言还是会继续执行，python-assume有助于我们进行多重校验，比assert更加高效