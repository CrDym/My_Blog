---
title: Pytest8-参数化
date: 2021-07-01  23:45
tags: Pytest
categories: Python学习

---

## 前言

我们在实际自动化测试中，某些测试用例是无法通过一组测试数据来达到验证效果的，所以需要通过参数化来传递多组数据

在unittest中，我们可以使用第三方库parameterized来对数据进行参数化，从而实现数据驱动测试

而在pytest中，也提供了功能强大的@pytest.mark.parametrize装饰器来实现数据参数化

## Pytest参数化的方式

pytest有三种传参方式

- `@pytest.mark.parametrize()`  通过装饰器方式进行参数化（最常使用）
- `pytest.fixture()`方式进行参数化，`fixture`装饰的函数可以作为参数传入其他函数
- `conftest.py`文件中存放参数化函数，可作用于模块内的所有测试用例

<!-- more -->



## @pytest.mark.parametrize实现参数化

### 装饰测试类

当装饰器 @pytest.mark.parametrize 装饰测试类时，会将数据集合传递给类的所有测试用例方法

举个🌰

```python
import pytest

# 定义测试数据
data1 = [
    (1, 2, 3),
    (4, 5, 9)
]


# 定义add方法
def add(a, b):
    return a + b


# 添加parametrize装饰器
@pytest.mark.parametrize('a, b, expect', data1)
class TestParametrize(object):

    def test_parametrize_1(self, a, b, expect):
        print(f'\n测试用例1数据为{a}-{b},结果为{expect}')
        assert add(a, b) == expect

    def test_parametrize_2(self, a, b, expect):
        print(f'\n测试用例2数据为{a}-{b},结果为{expect}')
        assert add(a, b) == expect
```

执行结果如下

![image-20210701160856783](https://img.rockche.cn//image-20210701160856783.png)

### 装饰测试函数

#### 单个数据

> 当测试用例只需要一个参数时，我们使用列表存放测试数据，例如定义一个列表
>
> data = [1，2]
>
> 使用@pytest.mark.parametrize装饰器时，第一个参数使用变量a接收列表中的每个元素，第二个参数传递存储数据的列表
>
> 在测试用例中使用同名的变量a接收测试数据，列表有多少个元素就会生成并执行多少个测试用例

上代码

```python
import pytest

data = [1, 2 , 3]


@pytest.mark.parametrize('a', data)
def test_parametrize(a):
    print(f'\n被加载测试数据为{a}')

```

执行结果如下

![image-20210701162111100](https://img.rockche.cn//image-20210701162111100.png)

#### 一组数据

> 当测试用例需要多个数据时，我们可以使用嵌套序列(嵌套元组&嵌套列表)的列表来存放测试数据
>
> 装饰器@pytest.mark.parametrize()可以使用单个变量接收数据，也可以使用多个变量接收，同样，测试用例函数也需要与其保持一致
>
> 当使用单个变量接收时，测试数据传递到测试函数内部时为列表中的每一个元素或者小列表，需要使用索引的方式取得每个数据
>
> 当使用多个变量接收数据时，那么每个变量分别接收小列表或元组中的每个元素
>
> 列表嵌套多少个列表或元组，测生成多少条测试用例

上代码

```python
import pytest

data = [
    [1, 2, 3],
    [4, 5, 9]
]


@pytest.mark.parametrize('a, b, expect', data)
def test_parametrize_1(a, b, expect):  
# 当使用多个变量接收数据时，那么每个变量分别接收小列表或元组中的每个元素
    print(f'\n测试数据为{a}，{b}，{expect}')
    actual = a + b
    assert actual == expect


@pytest.mark.parametrize('value', data)
def test_parametrize_2(value):
当使用单个变量接收时，测试数据传递到测试函数内部时为列表中的每一个元素或者小列表，需要使用索引的方式取得每个数据
    print(f'\n测试数据为{value}')
    actual = value[0] + value[1]
    assert actual == value[2]

```

执行结果如下

![image-20210701163343794](https://img.rockche.cn//image-20210701163343794.png)

#### 组合数据

> 一个测试函数还可以同时被多个参数化装饰器装饰，多个装饰器中的数据会进行交叉组合的方式传递给测试函数，进而生成n*n个测试用例，这也为我们的测试设计时提供了方便

上代码

```python
import pytest

data_1 = [1, 2]
data_2 = [3, 4, 5]


@pytest.mark.parametrize('a', data_1)
@pytest.mark.parametrize('b', data_2)
def test_parametrize_1(a, b):
    print(f'\n测试数据为{a}，{b}')
```

执行结果如下

![image-20210701164355094](https://img.rockche.cn//image-20210701164355094.png)

#### 标记用例

> 当我们不想执行某组测试数据时，我们可以标记skip或skipif
>
> 当我们预期某组数据会执行失败时，我们可以标记为xfail

上代码

```python
import pytest

data1 = [
    [1, 2, 3],
    pytest.param(3, 4, 8, marks=pytest.mark.xfail),
    pytest.param(3, 4, 7, marks=pytest.mark.skip)
]


def add(a, b):
    return a + b


@pytest.mark.parametrize("a,b,expected", data1)
def test_mark(a, b, expected):
    print(f'测试数据为{a},{b}，结果为{expected}')
    assert add(a, b) == expected
```

执行结果如下

![image-20210701233107572](https://img.rockche.cn//image-20210701233107572.png)

#### 嵌套字典

> 数据列表中也可以使用字典类型的数据

上代码

```python
import pytest

data_1 = (
    {
        'user': 1,
        'pwd': 2
     },
    {
        'user': 3,
        'pwd': 4
    }
)


@pytest.mark.parametrize('dic', data_1)
def test_parametrize_1(dic):
    print(f'测试数据为{dic}')
```

执行结果如下

![image-20210701233202805](https://img.rockche.cn//image-20210701233202805.png)

### 增加可读性

#### 使用ids参数

> 参数化装饰器有一个额外的参数ids，可以标识每一个测试用例，自定义测试数据结果的显示，用来增加测试用例的可读性

上代码

```python
import pytest

data = [1, 2, 3]

ids = [f'TestData-{a}' for a in data]

@pytest.mark.parametrize('a', data ,ids= ids)
def test_parametrize(a):
    print(f'\n被加载测试数据为  {a}')

```

执行结果为

![image-20210701234710810](https://img.rockche.cn//image-20210701234710810.png)

#### 自定义id做标识

除了使用ids参数增加输出可读性外，我们还可以在参数列表的参数旁边定义一个id值来做标识

上代码

```python
import pytest

data = [
pytest.param(1, id="this is test1"),
pytest.param(2, id="this is test2")
]



@pytest.mark.parametrize('a', data)
def test_parametrize(a):
    print(f'\n被加载测试数据为  {a}')
```

执行结果如下

![image-20210701235116235](https://img.rockche.cn//image-20210701235116235.png)

## pytest.fixture()方式进行参数化

待更新

## 整理参考

[linxu超](https://www.cnblogs.com/linuxchao/p/linuxchao-pytest-parametrize.html)