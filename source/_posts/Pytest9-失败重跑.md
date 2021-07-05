---
title: Pytest9-失败重跑
date: 2021-07-03  23:45
tags: Pytest
categories: Python学习

---

## 前言

在进行自动化测试的过程中，我们一定会有这样的需求：希望失败的用例可以自动重跑

在pytest中，提供了pytest-rerunfailures插件可以实现自动重跑的效果

## 插件安装

pip命令安装

```shell
pip install pytest-rerunfailures
```

## 使用实例

### 重新运行所有失败的用例

如果需要把所有失败的用例都重新运行，使用 --reruns 命令，并且制定要运行的最大次数

举个🌰：

```python
class TestDemo(object):
    def setup_class(self):
        print("执行setup_class")

    def teardown_class(self):
        print("执行teardown_class")

    def test_case1(self):
        print("执行测试用例1")
        assert 1 + 1 == 3

    def test_case2(self):
        print("执行测试用例2")
        assert 1 + 3 == 6

    def test_case3(self):
        print("执行测试用例3")
        assert 1 + 3 == 4
```

使用命令`pytest --reruns 2 -s` 执行，结果如下

![image-20210703221946118](https://img.rockche.cn//image-20210703221946118.png)

可以看到，case1和case2被重新执行了2次

如果希望在每次重新执行之间加上间隔时间，可以使用 `--reruns-delay` 命令行选项，指定下次测试重新开始之前等待的秒数

如 `pytest --reruns 2 --reruns-delay 5 -s`，代表自动重跑2次，每次间隔5s

### 重新运行指定的用例

如果我们在测试时，只希望在某一条测试用例失败后重新执行该如何处理呢

可以使用flaky装饰器 @pytest.mark.flaky(reruns=*, reruns_delay=*)

> 参数说明
>
> - reruns  重跑次数
> - reruns_delay 重新运行的等待时间

举个🌰

```python
import pytest


@pytest.mark.flaky(reruns=2, reruns_delay=3)
def test_case1():
    print("执行测试用例1")
    assert 1 + 1 == 3


def test_case2():
    print("执行测试用例2")
    assert 1 + 3 == 6


def test_case3():
    print("执行测试用例3")
    assert 1 + 3 == 4
```

运行结果如下：

![image-20210703223905568](https://img.rockche.cn//image-20210703223905568.png)

