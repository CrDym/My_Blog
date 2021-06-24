---
title: Pytest4-assert断言
date: 2021-06-24  23:45
tags: Pytest
categories: Python学习


---

## 前言

pytest作为单元测试框架，自然少不了断言功能，用过unittest的人都知道，在unittest中有丰富的断言方法，比如assertEqual()、assertIn()、assertTrue()、assertIs()等等，而在pytest中，并没有提供特殊的断言方法，而是直接使用python自带的关键字assert来进行断言操作。

下面我们就通过一些🌰来看看在pytest中是如何进行断言操作的吧

<!-- more -->

## 常用断言

`Pytest`里的断言实际上就是Python中的assert断言方法,常用断言方法如下：

> - assert xx ：判断 xx 为真
> - assert not xx ：判断 xx 不为真
> - assert a in b ：判断 b 包含 a
> - assert a == b ：判断 a 等于 b
> - assert a != b ：判断 a 不等于 b

举个🌰：

```python
import pytest


def test_demo1():
    a = 1
    assert a


def test_demo2():
    a = 0
    assert not a


def test_demo3():
    s = 'hello'
    assert 'h' in s


def test_demo4():
    a = 3
    assert a == 3


def test_demo5():
    a = 4
    assert a != 3


if __name__ == '__main__':
    pytest.main()
```

运行结果如下：

![image-20210624144052915](https://img.rockche.cn//image-20210624144052915.png)

如果想在异常的时候，能够输出一些提示信息，可在直接在断言后面加上提示信息，如下：

```python
import pytest

def test_demo6():
    a = 5
    assert a == 3, "两者不相等"
```

运行结果：

![image-20210624151716763](https://img.rockche.cn//image-20210624151716763.png)

## 异常断言

在实际测试的过程中，我们经常需要对特定异常进行断言，可以使用 pytest.raises 作为上下文管理器，当抛出异常时可以获取到对应的异常实例

举个🌰：

```python
import pytest


def test_zero_division():
    1 / 0


if __name__ == '__main__':
    pytest.main()
```

运行结果：

![image-20210624144815922](https://img.rockche.cn//image-20210624144815922.png)

可以看到，这里程序异常了，所以我们需要捕获并断言异常。

**断言场景**：断言抛出的异常是否符合预期。

**预期结果**：ZeroDivisionError: division by zero，其中ZeroDivisionError为错误类型，division by zero为具体错误值。

**断言方式**:  断言异常的type和value值。

**断言代码如下**：

```python
import pytest


def test_zero_division():
    with pytest.raises(ZeroDivisionError) as excinfo:
        1 / 0
    # 断言异常类型 type
    assert excinfo.type == ZeroDivisionError
    # 断言异常 value 值
    assert "division by zero" in str(excinfo.value)


if __name__ == '__main__':
    pytest.main()
```

> excinfo作为异常信息实例，拥有type 、value、.traceback等属性
>
> excinfo.value的值是元组，所以要转成字符串
>
> 在上下文管理器的作用域中，raises代码必须是最后一行，否则，其后面的代码将不会执行

## 拓展：match

你也可以给`pytest.raises()`传递一个关键字参数`match`，来测试异常的字符串表示`str(excinfo.value)`是否符合给定的正则表达式（和`unittest`中的`TestCase.assertRaisesRegexp`方法类似）：

```python
import pytest


def func():
    raise ValueError("Exception 123 raised")


def test_match():
  	# pytest.raises()函数，
    # 可以用元组的形式传递参数，只需要触发其中任意一个即可。
    # 通过match可以设置通过正则表达式匹配异常。
    with pytest.raises((ValueError, RuntimeError), match=r'.* 123 .*') as excinfo:
        func()
    assert “123” in str（excinfo.value）
    
if __name__ == '__main__':
    pytest.main()
```

## 拓展：检查断言装饰器

`pytest.mark.xfail()`也可以接收一个`raises`参数，来判断用例是否因为一个具体的异常而导致失败：

```python
@pytest.mark.xfail(raises=ZeroDivisionError)
def test_f():
    1 / 0
```

执行结果：

![image-20210624160307993](https://img.rockche.cn//image-20210624160307993.png)

> 如果`test_f()`触发的异常类型和raises指定的异常类型一致，则用例被标记为`xfailed`
>
> 如果`test_f()`测试成功，用例的结果是`xpassed`，而不是`passed`
>
> `pytest.raises`适用于检查由代码故意引发的异常；而`@pytest.mark.xfail()`更适合用于记录一些未修复的 Bug

## 参考

[pytest-chinese-doc](https://github.com/luizyao/pytest-chinese-doc/tree/6.1.1)

