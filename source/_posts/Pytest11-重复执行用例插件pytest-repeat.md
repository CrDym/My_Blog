---
title: Pytest11-重复执行用例插件pytest-repeat
date: 2021-07-13  23:45
tags: Pytest
categories: Python学习

---

## 前言

我们在平时做测试的时候，经常会遇到一些偶现的bug，通常我们会多次执行来复现此类bug，那么在自动化测试的时候，如何多次运行某个或某些用例呢，我们可以使用pytest-repeat这个插件来帮助我们重复的去执行用例

## pytest-repeat插件

### 插件安装

pip命令安装

```shell
pip install pytest-repeat
```

### 使用实例

上代码

```python
def test_demo1():
    print("执行测试用例1")


def test_demo2():
    print("执行测试用例2")
```

使用命令  `pytest -s --count 5  test_demo.py`执行

运行结果如下

![image-20210708175609335](https://img.rockche.cn//image-20210708175609335.png)

可以看到，用例被重复执行了5次

### 重复测试直到失败

> 当我们验证偶现的问题时，需要不停的重复执行用例，直到用例失败
>
> 可以将pytest的 -x 选项与pytest-repeat结合使用，以强制测试运行程序在第一次失败时停

上代码

```python
def test_example():
    import random
    flag = random.randint(1,100)
    print(flag)
    assert flag != 8
```

使用命令  `pytest --count 5 -x  test_demo.py`执行

运行结果如下

![image-20210714173546412](https://img.rockche.cn//image-20210714173546412.png)

可以看到，在运行了55次后，用例执行失败



### 标记要重复多次的测试

如果要在代码中将某些测试用例标记为执行重复多次，可以使用 `@pytest.mark.repeat(count)` 

上代码

```python
import pytest


def test_repeat1():
    print("测试用例1执行")


@pytest.mark.repeat(5)
def test_repeat2():
    print("测试用例2执行")


def test_repeat3():
    print("测试用例3执行")
```

使用命令  `pytest -s test_demo.py`执行

运行结果如下

![image-20210714174143630](https://img.rockche.cn//image-20210714174143630.png)

可以看到，用例2被执行了5次

### --repeat-scope

作用：类似于pytest fixture的scope参数，--repeat-scope也可以设置参数： `session` ， `module`，`class`或者`function`（默认值）

> - function：默认，范围针对每个用例重复执行，再执行下一个用例
> - class：以class为用例集合单位，重复执行class里面的用例，再执行下一个
> - module：以模块为单位，重复执行模块里面的用例，再执行下一个
> - session：重复整个测试会话，即所有测试用例的执行一次，然后再执行第二次

#### 重复执行class中的用例

上代码

```python
class Test1:
    def test_repeat1(self):
        print("测试用例执行1")

class Test2:
    def test_repeat2(self):
        print("测试用例执行2")
```

使用命令  ` pytest -s --count=2 --repeat-scope=class test_demo.py`执行

运行结果如下

![image-20210714174917068](https://img.rockche.cn//image-20210714174917068.png)

可以看到，两个测试类都执行了2次

#### 重复执行module中的用例

上代码

```python
class Test1:
    def test_repeat1(self):
        print("测试用例执行1")

class Test2:
    def test_repeat2(self):
        print("测试用例执行2")

def test_repeat3():
    print("测试用例执行3")
```

使用命令  ` pytest -s --count=2 --repeat-scope=module test_demo.py`执行

运行结果如下

![image-20210714175321251](https://img.rockche.cn//image-20210714175321251.png)

### 注意

pytest-repeat不能与unittest.TestCase测试类一起使用。无论--count设置多少，这些测试始终仅运行一次，并显示警告

### 整理参考

[小菠萝测试笔记](https://www.cnblogs.com/poloyy/p/12691240.html)

