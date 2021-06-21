---
title: Pytest2-setup和teardown
date: 2021-06-21  23:45
tags: Pytest
categories: Python学习

---



# 前言

我们在做自动化的时候，常常有这样的需求：

- 执行每一条用例时，都重新启动一次浏览器

- 每一条用例执行结束时，都清除测试数据

在unittest中，我们可以使用 setUp() 和 tearDown() 两个方法来实现以上需求，其中 setUp() 方法用于初始化测试固件；而 tearDown() 方法用于销毁测试固件。程序会在运行每个测试用例（以 test_ 开头的方法）之前自动执行 setUp() 方法来初始化测试固件，井在每个测试用例（以 test_ 开头的方法）运行完成之后自动执行 tearDown() 方法来销毁测试固件。

那么如何实现只启动一次浏览器，所有测试用例执行结束后再清除数据这样的需求呢？

- unittest提供了setUpClass()和tearDownClass()两个方法，配合@classmethod装饰器使用即可

作为比unittest更强大的框架，pytest自然也有类似的方法

<!-- more -->
pytest的setup/teardown方法包括：

- 模块级别(setup_module/teardown_module)
- 函数级别(setup_function/teardown_function)
- 类级别(setup_class/ teardown_class)
- 方法级别(setup_method/teardown_methond或者setup/teardown)

# 模块级别

模块中的第一个用例开始前执行一次setup_module方法，模块中的最后一个测试用例结束后执行一次teardown_module方法

```python
import pytest


def setup_module():
    print("执行setup_module")


def teardown_module():
    print("执行teardown_module")


class TestDemo(object):
    def test_case1(self):
        print("执行测试用例1")
        assert 1 + 1 == 2

    def test_case2(self):
        print("执行测试用例2")
        assert 1 + 3 == 4

    def test_case3(self):
        print("执行测试用例3")
        assert 1 + 5 == 6
```

运行结果如下：

![image-20210621231421826](https://img.rockche.cn//image-20210621231421826.png)

# 函数级别

在每个测试函数前运行一次setup_function方法，在每个测试函数结束后运行一次teardown_function方法，只对函数用例生效，不在类中。

```python
import pytest


def setup_function():
    print("执行setup_function")


def teardown_function():
    print("执行teardown_function")


def test_case1():
    print("执行测试用例1")
    assert 1 + 1 == 2


def test_case2():
    print("执行测试用例2")
    assert 1 + 3 == 4


def test_case3():
    print("执行测试用例3")
    assert 1 + 5 == 6
```

运行结果如下：

![image-20210621231355291](https://img.rockche.cn//image-20210621231355291.png)

# 类级别

setup_class/teardown_class 对类有效，位于类中，在执行测试类之前和之后各调用一次

```python
import pytest


class TestDemo(object):
    def setup_class(self):
        print("执行setup_class")

    def teardown_class(self):
        print("执行teardown_class")

    def test_case1(self):
        print("执行测试用例1")
        assert 1 + 1 == 2

    def test_case2(self):
        print("执行测试用例2")
        assert 1 + 3 == 4

    def test_case3(self):
        print("执行测试用例3")
        assert 1 + 5 == 6
```

运行结果如下：

![image-20210621231310442](https://img.rockche.cn//image-20210621231310442.png)

# 方法级别

setup_method/teardown_method和setup/teardown，在测试类中每个测试方法前后调用一次。这两个方法效果是一样的

```python
import pytest


class TestDemo(object):
    def setup_method(self):
        print("执行setup_method")

    def teardown_method(self):
        print("执行teardown_method")

    def test_case1(self):
        print("执行测试用例1")
        assert 1 + 1 == 2

    def test_case2(self):
        print("执行测试用例2")
        assert 1 + 3 == 4

    def test_case3(self):
        print("执行测试用3")
        assert 1 + 5 == 6
```

运行结果如下：

![image-20210621231121966](https://img.rockche.cn//image-20210621231121966.png)

# 四种级别混合使用

如果把这四种级别的方法混合使用，运行顺序如何呢？

```python
import pytest


def setup_module():
    print("模块开始时，执行setup_module")


def teardown_module():
    print("模块结束时，执行teardown_module")


def setup_function():
    print("函数用例开始时，执行setup_function")


def teardown_function():
    print("函数用例结束时，执行teardown_function")


def test_a():
    print("执行测试函数a")


def test_b():
    print("执行测试函数b")


class TestDemo(object):
    def setup_class(self):
        print("测试类开始时，执行setup_class")

    def teardown_class(self):
        print("测试类结束时，执行teardown_class")

    def setup_method(self):
        print("类中的方法开始时，执行setup_method")

    def teardown_method(self):
        print("类中的方法结束时，执行teardown_method")

    def test_case1(self):
        print("执行测试用例1")
        assert 1 + 1 == 2

    def test_case2(self):
        print("执行测试用例2")
        assert 1 + 3 == 4

    def test_case3(self):
        print("执行测试用例3")
        assert 1 + 5 == 6
```



运行结果如下：

![image-20210621233905952](https://img.rockche.cn//image-20210621233905952.png)

# 总结

- 模块级（setup_module/teardown_module）开始于模块始末，全局的
- 函数级（setup_function/teardown_function）只对函数用例生效（不在类中）
- 类级（setup_class/teardown_class）只在类中前后运行一次(在类中)
- 方法级（setup_method/teardown_method或setup/teardown）开始于方法始末（在类中）

