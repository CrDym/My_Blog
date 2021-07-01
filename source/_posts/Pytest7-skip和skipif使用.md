---
title: Pytest7-skip和skipif使用
date: 2021-06-30  23:45
tags: Pytest
categories: Python学习

---

## 前言

在实际的测试中，我们经常会遇到需要跳过某些测试用例的情况，pytest提供了skip和ifskip来跳过测试

下面我们就来通过一些例子看看skip和ifskip具体如何使用吧

## skip的用法

使用示例：@pytest.mark.skip(reason="跳过的原因，会在执行结果中打印")

<!-- more -->

### 标记在测试函数中

举个🌰

```python
import pytest


def test_1():
    print("测试用例1")


@pytest.mark.skip(reason="没写完，不执行此用例")
def test_2():
    print("测试用例2")
```

执行结果如下：

![image-20210630114004795](https://img.rockche.cn//image-20210630114004795.png)

### 标记在测试类的测试用例中

举个🌰

```python
import pytest

class TestCase(object):
    def test_1(self):
        print("测试用例1")


    @pytest.mark.skip(reason="没写完，不执行此用例")
    def test_2(self):
        print("测试用例2")
```

执行结果如下

![image-20210630114543144](https://img.rockche.cn//image-20210630114543144.png)

### 标记在测试类方法上

举个🌰

```python
import pytest


@pytest.mark.skip(reason="没写完，不执行此用例")
class TestCase1(object):
    def test_1(self):
        print("测试用例1")

    def test_2(self):
        print("测试用例2")


class TestCase2(object):
    def test_3(self):
        print("测试用例3")

    def test_4(self):
        print("测试用例4")
```

执行结果如下

![image-20210630134802163](https://img.rockche.cn//image-20210630134802163.png)

总结

> - @pytest.mark.skip 可以加在函数上，类上，类方法上
> - 如果加在类上面，则类里面的所有测试用例都不会执行

### 在测试用例执行期间强制跳过

以一个for循环为例，执行到第3次的时候跳出

```python
import pytest

def test_demo():
    for i in range(50):
        print(f"输出第【{i}】个数")
        if i == 3:
            pytest.skip("跑不动了，不再执行了")
```

执行结果如下

![image-20210630140439318](https://img.rockche.cn//image-20210630140439318.png)

### 在模块级别跳过测试用例

语法：`pytest.skip(msg="",allow_module_level=False)`

当`allow_module_level=True`时，可以设置在模块级别跳过整个模块

```Python
import pytest

pytest.skip("跳过整个模块", allow_module_level=True)

@pytest.fixture(autouse=True)
def test_1():
    print("执行测试用例1")

def test_2():
    print("执行测试用例2")
```

执行结果如下

![image-20210630141608529](https://img.rockche.cn//image-20210630141608529.png)

### 有条件的跳过某些用例

语法：@pytest.mark.skipif(condition, reason="")

```python
import sys
import pytest


@pytest.mark.skipif(sys.platform == 'darwin', reason="does not run on MacOS")
class TestSkipIf(object):
    def test_demo(self):
        print("不能在MacOS上运行")
```

**注意：**condition需要返回True才会跳过

执行结果如下：

![image-20210630142429456](https://img.rockche.cn//image-20210630142429456.png)



### 跳过标记的使用

- 可以将 pytest.mark.skip 和 pytest.mark.skipif 赋值给一个标记变量
- 在不同模块之间共享这个标记变量
- 若有多个模块的测试用例需要用到相同的 skip 或 skipif ，可以用一个单独的文件去管理这些通用标记，然后适用于整个测试用例集

举个🌰

```python
import sys
import pytest

skipmark = pytest.mark.skip(reason="不执行此用例")
skipifmark = pytest.mark.skipif(sys.platform == 'darwin', reason="does not run on MacOS")


@skipifmark
class TestSkipIf(object):
    def test_demo(self):
        print("不能在MacOS上运行")


@skipmark
def test_1():
    print("测试用例1")


def test_2():
    print("测试用例2")
```

执行结果如下

![image-20210630145126039](https://img.rockche.cn//image-20210630145126039.png)

### 当缺少某些导入时跳过用例

语法：

`pytest.importorskip( modname: str, minversion: Optional[str] = None, reason: Optional[str] = None )`

参数：

- modname： 需要被导入的模块名称，比如 selenium；
- minversion： 表示需要导入的最小的版本号，如果该版本不达标，将会打印出报错信息；
- reason： 只有当模块没有被导入时，给定该参数将会显示出给定的消息内容

#### 找不到对应module

举个🌰

```python
import pytest
rock = pytest.importorskip("rock")

@rock
def test_1():
    print("测试是否导入了rock模块")
```

运行结果

![image-20210630151602831](https://img.rockche.cn//image-20210630151602831.png)

#### 如果版本不达标

举个🌰

```python
import pytest
sel = pytest.importorskip("selenium", minversion="3.150")

@sel
def test_1():
  	print("测试是否导入了selenium模块")
```

运行结果

![image-20210630151755943](https://img.rockche.cn//image-20210630151755943.png)

###  整理参考

[小菠萝的测试笔记](https://www.cnblogs.com/poloyy/p/12666682.html)