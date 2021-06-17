---
title: Pytest学习笔记
date: 2021-06-16  20:12
tags: Pyttest
categories: Python学习
---

# Pytest特点

Pytest是Python的一个第三方单元测试库。它的目的是让单元测试变得更容易，并且也能扩展到支持应用层面复杂的功能测试。

Pytest的特点有：

- 入门简单，易上手，文档丰富
- 支持用简单的assert语句实现丰富的断言，无需复杂的self.assert*函数
- 支持参数化
- 自动识别测试模块和测试函数
- 执行测试过程中可以将某些测试跳过（skip），或者对某些预期失败的case标记成失败
- 支持重复执行(rerun)失败的 case
- 模块化夹具用以管理各类测试资源
- 对 unittest 完全兼容
- 可以很好的和jenkins集成
- 丰富的插件生态，有各式各样的插件，社区繁荣
 <!-- more -->

# 快速入门

## 安装pytest

使用 pip 进行安装：

```shell
pip install pytest
```

安装之后查看版本：

```shell
pytest --version
```

## 快速开始

### 1.创建测试函数

Pytest 使用 Python 的 `assert` 进行条件判断，最简单的测试函数如：

```python
# testdemo.py

def test_passing():
	assert(1,2,3) ==(1,2,3)
```

### 2.运行测试函数

在当前文件目录使用命令`pytest`运行测试函数：

![image-20210616223825437](https://img.rockche.cn/mweb/image-20210616223825437.png)

### 3.pytest运行规则

pytest会查找当前目录及其子目录下以`test_`开头的文件或以`_test`结尾的文件，找到文件后，执行文件中以test开头的 函数

### 4.创建测试类

```python
# test_class.py

class TestClass:
	def test_a(self):
    x = "hello"
    assert "h" in x
  def test_b(self):
    x = "hello"
    assert "a" in x
```

在当前文件目录使用命令`pytest`运行测试：

![image-20210617002840628](https://img.rockche.cn/mweb/image-20210617002840628.png)

第一次测试通过，第二次测试失败。 可以在断言中查看失败的原因



## Pytest执行用例命令行参数

除了`pytest`命令，pytest还提供了很多执行用例的命令行参数，如下：

### 1.-v

输出用例更加详细的执行信息，比如用例所在的文件及用例名称等

![image-20210617141445528](https://img.rockche.cn/mweb/image-20210617141445528.png)

### 2.-s

输出用例中的调式信息，比如print的打印信息等，我们在之前的test_class.py第3行加上一句 `print(“测试调试信息”)`

```python
# test_class.py

class TestClass:
	def test_a(self):
		print("测试调试信息")
    x = "hello"
    assert "h" in x
  def test_b(self):
    x = "hello"
    assert "a" in x
```

   再执行一次用例看看

   ![image-20210617141824233](https://img.rockche.cn/mweb/image-20210617141824233.png)

### 3.-m

执行被标记的测试用例

我们修改test_class.py文件如下：

```python
# test_class.py

import pytest
class TestClass:
	def test_a(self):
		print("测试调试信息")
    x = "hello"
    assert "h" in x
    
  @pytest.mark.run_this_case
  def test_b(self):
    x = "hello"
    assert "a" in x
```

使用命令 `pytest -m run_this_case  test_calss.py`执行

![image-20210617144012907](https://img.rockche.cn/mweb/image-20210617144012907.png)

可以看到只执行了被标记的用例

### 4.-k

执行用例包含“关键字”的用例

我们修改test_class.py文件如下：

```python
# test_class.py

import pytest
class TestClass:
	def test_a(self):
		print("测试调试信息")
    x = "hello"
    assert "h" in x
    
  def test_b(self):
    x = "hello"
    assert "a" in x
    
  def test_rock(self):
    x = "rock"
    assert "o" in x
```

使用命令 `pytest -k "rock" test_class.py`执行

![image-20210617144500945](https://img.rockche.cn/mweb/image-20210617144500945.png)

### 5.-q

简化控制台的输出，可以看到输出信息和上面的结果都不一样， 下图中有三个.代替了pass结果

![image-20210617145043766](https://img.rockche.cn/mweb/image-20210617145043766.png)

### 6.--collect-only

罗列出所有当前目录下所有的测试模块，测试类及测试函数

![image-20210617145514237](https://img.rockche.cn/mweb/image-20210617145514237.png)

### 7.--tb=style

执行用例的时候，有些用例执行失败的时候，屏幕上会出现一大堆的报错内容，不方便快速查看是哪些用例失败，`--tb=style`可以屏蔽测试用例执行输出的回溯信息，可以简化用例失败时的输出信息, style的选项有['auto', 'long', 'short', 'no', 'line', 'native'], 具体区别如下：

--tb=auto 有多个用例失败的时候，只打印第一个和最后一个用例的回溯信息
--tb=long 输出最详细的回溯信息
--tb=short 输入assert的一行和系统判断内容
--tb=line 使用一行显示错误信息
--tb=native 只输出python标准库的回溯信息
--tb=no 不显示回溯信息

### 8.--lf和--ff

--last-failed 只重新运行上次运行失败的用例（或如果没有失败的话会全部跑）

--failed-first 运行所有测试，但首先运行上次运行失败的测试（这可能会重新测试，从而导致重复的fixture setup/teardown）

我们修改test_class.py文件如下：

```python
# test_class.py

import pytest
class TestClass:
	def test_a(self):
		print("测试调试信息")
    x = "hello"
    assert "h" in x
    
  def test_b(self):
    x = "hello"
    assert "a" in x
    
  def test_rock(self):
    x = "rock"
    assert "o" in x
    
  def test_c(self):
    x = "world"
    assert "q" in x
```

使用命令`pytest`执行全部用例，结果如下：

![image-20210617151647993](https://img.rockche.cn/mweb/image-20210617151647993.png)

可以看到执行了4条用例，失败了2条

然后使用命令`pytest --lf`，执行失败的用例，结果如下：

![image-20210617151841294](https://img.rockche.cn/mweb/image-20210617151841294.png)

可以看到只执行了两条失败的用例

最后我们使用命令`pytest --ff`，先执行失败的用例，再执行成功的用例，结果如下：

![image-20210617152006219](https://img.rockche.cn/mweb/image-20210617152006219.png)

可以看到先执行了2条失败的用例，然后执行了2条成功的用例



以上就是命令行运行测试用例时经常使用到的参数，这些参数不仅可以单独使用，也可以组合一起使用，可以使用`--help`来查看更多命令的帮助信息



## Pytest收集测试用例的规则

- 从一个或者多个目录开始查找，你可以在命令行指定文件或者目录，如果未指定那么从当前目录开始收集用例
- 在该目录和所有子目录下递归查找测试模块
- 测试模块是指文件名为test_*.py或者*_test.py的文件
- 在测试模块中查找以test_开头的函数
- 查找名字以Test开头的类。其中首先筛选掉包含__init__()函数的类，再查找类中以Test_开头的类方法

### 规则验证

我们按照以下目录结构新建一个项目

![image-20210617155429499](https://img.rockche.cn/mweb/image-20210617155429499.png)

代码如下：

case01.py

```python
# case01.py
# 测试函数
def test_02():
    assert 1 == 1


# 普通函数
def func():
    assert 1 == 1
```

test_01.py
```python
# test_01.py
# 测试函数
def test_01():
    assert 1 == 1


# 普通函数
def func_01():
    print("这是一个普通函数")


# 测试类
class TestClass_01(object):
    # 测试函数
    def test_class_1(self):
        assert 1 == 1

    # 普通函数
    def func_class_1(self):
        assert 1 == 1


# 普通类
class NoTestClass_02(object):
    # 测试函数
    def test_class_2(self):
        assert 1 == 1

    # 普通函数
    def func_class_2(self):
        assert 1 == 1
```

case.py

```python
# 测试函数
def test_02():
    assert 1 == 1


# 普通函数
def func():
    assert 1 == 1
```

在项目根目录下执行`pytest --collect-only`,结果如下：

![image-20210617160120440](https://img.rockche.cn/mweb/image-20210617160120440.png)

可以看到，只有2条有效的测试用例

使用`pytest -v`命令执行用例，结果如下：

![image-20210617160637211](https://img.rockche.cn/mweb/image-20210617160637211.png)

只有test_01.py被识别为测试模块

