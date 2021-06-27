---
title: Pytest5-conftest.py的用法
date: 2021-06-26  23:45
tags: Pytest
categories: Python学习
---



## 前言

在之前介绍fixture的文章中，我们使用到了conftest.py文件，那么conftest.py文件到底该如何使用呢，下面我们就来详细了解一下conftest.py文件的特点和使用方法吧

## 什么是conftest.py

我们之前了解了fixture，fixture可以直接定义在测试脚本中，但是有些时候，我们希望一个fixture可以被复用，这就需要对fixture进行集中管理，Pytest使用文件`conftest.py`集中管理固件.在复杂的项目中,可以在不同的目录层级定义conftest.py,其作用域为其所在的目录和子目录，通常情况下，`conftest.py`和`@pytest.fixture()`会结合使用，来实现全局的前后置处理。

<!-- more -->

## conftest.py特点

- `conftest.py`文件的名称是固定的，不能修改
- `conftest.py`与运行的用例要在同一个pakage下，并且有`__init__.py`文件
- 不需要`import`导入`conftest.py`文件，pytest用例会自动识别该文件，放到根目录下可以全局目录调用，放在某个package下，那就在该package内有效
- 不同目录可以有自己的conftest.py，一个项目中可以有多个`conftest.py`
- pytest会默认读取`conftest.py`里面的所有fixture，所有同目录测试文件运行前都会执行`conftest.py`文件

## conftest.py用法

在我们实际的测试中，conftest.py文件需要结合fixture来使用，所以fixture中参数scope也适用conftest.py中fixture的特性，这里再说明一下

> - conftest中fixture的scope参数为session，所有的测试文件执行前（后）执行一次`conftest.py`文件中的fixture。
> - conftest中fixture的scope参数为module，每一个测试.py文件执行前（后）都会执行一次`conftest.py`文件中的fixture
> - conftest中fixture的scope参数为class，每一个测试文件中的测试类执行前（后）都会执行一次`conftest.py`文件中的fixture
> - conftest中fixture的scope参数为function，所有文件的测试用例执行前（后）都会执行一次`conftest.py`文件中的fixture

## conftest.py实际案例

我们按照这样的目录新建一个项目

![image-20210627112906673](https://img.rockche.cn//image-20210627112906673.png)

### 在根目录conftestdemo下

根目录中的conftest.py文件中，一般写全局的fixture，比如登录

conftest.py

```python
import pytest


@pytest.fixture(scope="session")
def login():
    print("***登录成功，返回用户名***")
    name = "rockche"
    yield name
    print("***退出登录***")


@pytest.fixture(autouse=True)
def get_name(login):
    name = login
    print(f"--每个用例都调用外层fixiture：打印用户name：{name}--")
```

根目录下的测试用例

test_1.py

```python
def test_get_name(login):
    name = login
    print("***基础用例：获取用户name***")
    print(f"用户名:{name}")

```

运行conftestdemo下的所有用例

run.py

```python
import pytest

if __name__ == '__main__':
    pytest.main(["-s", "../conftestdemo/"])
```

### test_baidu目录下

配置针对baidu网站的测试用例独有的fixture

conftest.py

```python
import pytest


@pytest.fixture(scope="module")
def open_baidu(login):
    name = login
    print(f"用户 {name} 打开baidu")

```

test_case1.py

```python
def test_case2_01(open_baidu):
    print("搜索pytest")


def test_case2_02(open_baidu):
    print("搜索博客园")

```

### test_cnblogs目录下

没有`__init__.py`文件也没有conftest.py文件

test_case1.py

```python
def test_no_fixture(login):
    print("没有__init__文件，直接进入cnblogs", login)
```

### test_taobao目录下

配置针对taobao网站的测试用例独有的fixture

conftest.py

```python
import pytest


@pytest.fixture(scope="function")
def open_taobao(login):
    name = login
    print(f"用户 {name} 进入淘宝")
```

test_case1.py

```python
class TestTaobao:
    def test_case1_01(self, open_taobao):
        print("选购商品")

    def test_case1_02(self, open_taobao):
        print("进入结算界面")
```

### 运行run.py

![image-20210627115527210](https://img.rockche.cn//image-20210627115527210.png)



## 整理参考

[小菠萝的测试笔记](https://www.cnblogs.com/poloyy/p/12663601.html)

