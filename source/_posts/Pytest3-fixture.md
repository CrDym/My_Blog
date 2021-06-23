---
title: Pytest3-fixture
date: 2021-06-22  23:45
tags: Pytest
categories: Python学习


---

# 前言

个人认为，fixture是pytest最精髓的地方，也是学习pytest必会的知识点。



# fixture用途

- 用于执行测试前后的初始化操作，比如打开浏览器、准备测试数据、清除之前的测试数据等等
- 用于测试用例的前置条件，比如UI自动化的登录操作，读取config参数等
- 用于测试用例之间的参数和数据传递

# fixture优势

firture相对于`unittest`中的setup和teardown来说应该有以下几点优势

- 命名方式更加的灵活，不局限于setup和teardown
- conftest.py 配置里可以实现数据共享，不需要import就能自动找到一些配置
- scope="module" 可以实现多个.py跨文件共享前置, 每一个.py文件调用一次
- scope="session" 以实现多个.py跨文件使用一个session来完成多个用例

# fixture语法

```python
fixture(callable_or_scope=None, *args, scope="function", params=None, autouse=False, ids=None, name=None)
```

- scope：fixture的作用域，默认为function

- autouse：默认为False，表示需要用例手动调用该fixture；当为True时，表示所有作用域内的测试用例都会自动调用该fixture

- name：装饰器的名称，同一模块的fixture相互调用建议使用不同的name

  <!-- more -->

# fixture定义

fixture通过`@pytest.fixture()`装饰器装饰一个函数，这个函数就是一个fixture，实例如下：

```python
# test_fixture.py

import pytest


@pytest.fixture()
def fixtureDemo():
    return "一个fixture"


def test_fixture(fixtureDemo):
    print("测试用例执行时调用了{}".format(fixtureDemo))


if __name__ == "__main__":
    pytest.main(['-v', 'test_fixture.py'])
```



执行结果如下：

![image-20210622175407924](https://img.rockche.cn//image-20210622175407924.png)

# fixture调用

调用fixture有三种方式

- fixture名字作为用例函数的参数
- 使用@pytest.mark.usefixtures('fixture名称')装饰器
- 使用autouse参数

## fixture名字作为用例函数的参数

将fixture名称作为参数传入测试用例，如果fixture有返回值，那么测试用例将会接收返回值

举个🌰：

```python
import pytest


@pytest.fixture()
def fixtureDemo():
    return '打开浏览器'


def test_fixture(fixtureDemo):
    print('执行测试用例的时候，先{}'.format(fixtureDemo))


class TestFixture(object):
    def test_fixture_class(self, fixtureDemo):
        print('在类中执行测试用例的时候，先 "{}"'.format(fixtureDemo))
```

执行结果如下：

![image-20210622180857595](https://img.rockche.cn//image-20210622180857595.png)

## 使用@pytest.mark.usefixtures('fixture名称')装饰器

每个函数或者类前使用@pytest.mark.usefixtures('fixture名称')装饰器装饰

举个🌰：

```python
import pytest


@pytest.fixture()
def fixtureDemo():
    print('执行fixture')


@pytest.mark.usefixtures('fixtureDemo')
def test_fixture():
    print('执行测试用例')


@pytest.mark.usefixtures('fixtureDemo')
class TestFixture(object):
    def test_fixture_class(self):
        print('在类中执行测试用例')

```

执行结果如下：

![image-20210622220259171](https://img.rockche.cn//image-20210622220259171.png)

##  使用autouse参数

指定fixture的参数autouse=True，这样同一作用域的每个测试用例会自动调用fixture，autouse为False时，需要手动调用fixture

举个🌰：

```python
import pytest


@pytest.fixture(autouse=True)
def fixtureDemo():
    print('执行fixture')



def test_fixture():
    print('执行测试用例')


class TestFixture(object):
    def test_fixture_class(self):
        print('在类中执行测试用例')
```



执行结果如下：

![image-20210622220821598](https://img.rockche.cn//image-20210622220821598.png)

## 三种方式的区别

以上便是fixture的三种调用方式，那么这三种方式有何不同呢，观察🌰，可以看出：

如果在测试用例中需要使用到fixture中返回的参数，就只能使用第一种调用方式了，因为fixture中返回的数据是默认在fixture名字中存储的。

# fixture作用范围

fixture参数中的`scope`参数可以控制fixture的作用范围，scope参数可以是session， module，class，function， 默认为function

- session 会话级别：是多个文件调用一次，可以跨.py文件调用，每个.py文件就是module（通常配合conftest.py文件使用）；
- module 模块级别：模块里所有的用例执行前执行一次module级别的fixture；
- class 类级别 ：每个类执行前都会执行一次class级别的fixture；
- function  函数级别：每个测试用例执行前都会执行一次function级别的fixture（前面的🌰）

**说明：当 fixture 有返回值时，pytest 会把返回值缓存起来，如果 fixture 在指定的作用域内被多次调用，只有第一次调用会真正的被执行，后续调用会使用被缓存起来的返回值，而不是再执行一遍；**

下面我们通过实例来对fixture的作用范围进行了解

## function级别

每个测试用例之前运行一次

举个🌰：

```python
import pytest


@pytest.fixture()
def fixtureDemo():
    return "fixture"


def test_01(fixtureDemo):
    print("测试用例1执行时调用了{}".format(fixtureDemo))

def test_02(fixtureDemo):
    print("测试用例2执行时调用了{}".format(fixtureDemo))
```

运行结果如下：

![image-20210623000220362](https://img.rockche.cn//image-20210623000220362.png)

## class级别

如果一个class里面有多个用例，都调用了此fixture，那么fixture只在此class里所有用例开始前执行一次

举个🌰：

```python
import pytest


@pytest.fixture(scope="class")
def fixtureDemo():
    print('执行fixture')


@pytest.mark.usefixtures('fixtureDemo')
class TestFixture(object):
    def test_01(self):
        print('在类中执行测试用例1')

    def test_02(self):
        print('在类中执行测试用例2')
```

运行结果如下：

![image-20210623001002710](https://img.rockche.cn//image-20210623001002710.png)

## module级别

在当前.py脚本里面所有用例开始前只执行一次

举个🌰：

```python
import pytest


@pytest.fixture(scope="module")
def fixtureDemo():
    print('执行fixture')


@pytest.mark.usefixtures('fixtureDemo')
def test_01():
    print("执行测试用例1")


@pytest.mark.usefixtures('fixtureDemo')
class TestFixture(object):
    def test_02(self):
        print('在类中执行测试用例2')

    def test_03(self):
        print('在类中执行测试用例3')
```

运行结果如下：

![image-20210623003645546](https://img.rockche.cn//image-20210623003645546.png)

## session级别

session级别是可以跨模块调用的，如果多个模块下的用例只需调用一次fixture，可以设置scope="session"，并且写到conftest.py文件里。

conftest.py作用域：放到项目的根目录下就可以全局调用了，如果放到某个package下，那就在该package内有效。

conftest.py的fixture调用方式，无需导入，直接使用

举个🌰：

先新建一个conftest.py文件

```python
import pytest


@pytest.fixture()
def fixturedemo():
    print("执行fixture")
```



test_01.py

```python
def test_01(fixturedemo):
    print('执行测试用例1')
```

test_02.py

```python
def test_02(fixturedemo):
    print('执行测试用例2')
```

在当前目录中使用命令 `pytest -v -s` 执行，结果如下：

![image-20210623143540941](https://img.rockche.cn//image-20210623143540941.png)

可以看到，在执行不同模块中的测试用例前，都调用了fixture

## 案例说明

看完有点懵？我们再用一个🌰来看看

在3个测试方法中同时调用4个级别的fixture来看看效果

先新建一个conftest.py文件

```python
import pytest


# 作用域 function
@pytest.fixture(scope='function')
def fix_func():
    print('方法级：fix_func')


# 作用域 class
@pytest.fixture(scope='class')
def fix_class():
    print('类级：fix_class')


# 作用域 module
@pytest.fixture(scope='module')
def fix_module():
    print('模块级：fix_module')


# 作用域 session
@pytest.fixture(scope='session')
def fix_session():
    print('会话级：fix_session')
```

test_demo.py

```python
import pytest


class TestClass_1:
    """测试类1"""

    @pytest.mark.usefixtures('fix_func')
    @pytest.mark.usefixtures('fix_class')
    @pytest.mark.usefixtures('fix_module')
    @pytest.mark.usefixtures('fix_session')
    def test_func_1(self):
        """测试方法1"""
        print("测试方法1")

    @pytest.mark.usefixtures('fix_func')
    @pytest.mark.usefixtures('fix_class')
    @pytest.mark.usefixtures('fix_module')
    @pytest.mark.usefixtures('fix_session')
    def test_func_2(self):
        """测试方法2"""
        print("测试方法2")


class TestClass_2:
    """测试类2"""

    @pytest.mark.usefixtures('fix_func')
    @pytest.mark.usefixtures('fix_class')
    @pytest.mark.usefixtures('fix_module')
    @pytest.mark.usefixtures('fix_session')
    def test_func_3(self):
        """测试方法3"""
        print("测试方法3")

```

执行结果如下：

![image-20210623153946395](https://img.rockche.cn//image-20210623153946395.png)

观察运行结果，我们可以发现：

> - test_func_1 的调用全部被执行了；
>
> - test_func_2 的调用只有 function 级的被执行，因为 test_func_2 和 test_func_1 同属于同一个 session、module、class，所以这三个都不执行，只有 function 执行；
>
> - test_func_3 的调用执行了类级和方法级，因为 test_func_3 属于 另外一个类，所以 class 级会被再次调用

# fixture关键字yield

前面讲的是在用例前加前置条件，相当于setup,那么类似teardown的功能如何使用fixture实现呢，我们可以使用yield关键字

yield关键字的作用其实和函数中的return关键字差不多，可以返回数据给调用者，唯一的不同是当函数执行遇到yield时，会停止执行，然后执行调用处的函数，调用的函数执行完后会继续执行yield关键后面的代码

## yield实现teardown

举个🌰：

```python
import pytest


@pytest.fixture(scope="module")
def fixtureDemo():
    print('执行fixture')
    yield
    print('执行teardown操作')


def test_01(fixtureDemo):
    print('执行测试用例1')


def test_02(fixtureDemo):
    print('执行测试用例2')
```

运行结果如下：

![image-20210623151242016](https://img.rockche.cn//image-20210623151242016.png)

## yield遇到异常

如果多个用例运行时，有一个用例出现异常，不会影响yield后面的代码执行 , 运行结果互不影响，当全部用例执行完之后，yield后面的代码会正常执行

举个🌰：

```python
import pytest


@pytest.fixture(scope="module")
def fixtureDemo():
    print('执行fixture')
    yield
    print('执行teardown操作')


def test_01(fixtureDemo):
    print('执行测试用例1')
    raise NameError # 模拟异常


def test_02(fixtureDemo):
    print('执行测试用例2')
```

运行结果如下：

![image-20210623151912711](https://img.rockche.cn//image-20210623151912711.png)

