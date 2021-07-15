---
title: Pytest12-配置文件pytest.ini
date: 2021-07-15  22:45
tags: Pytest
categories: Python学习
---

## 前言

pytest配置文件可以改变pytest的运行方式，它是一个固定的文件pytest.ini文件，读取配置信息，按指定的方式去运行。

## 常用的配置项

### marks

作用：测试用例中添加了自定义标记（ @pytest.mark.xxx 装饰器），如果不添加marks选项的话，就会报warnings

格式：list列表类型

写法：

```ini
[pytest]
# 自定义标记说明
markers =
    tencent: tencent
    toutiao: toutiao
    alibaba: alibaba
```

### xfail_strict

作用：设置xfail_strict = True可以让那些标记为@pytest.mark.xfail但实际通过显示XPASS的测试用例被报告为失败

格式：True 、False（默认），1、0

写法：

```ini
[pytest]

# 自定义标记说明
markers =
    tencent: tencent
    toutiao: toutiao
    alibaba: alibaba

xfail_strict = True
```

上代码

```python
@pytest.mark.xfail()
def test_case1():
    a = "a"
    b = "b"
    assert a != b
```



- 未设置 xfail_strict = True 时，测试结果显示xpassed

![image-20210715165036370](https://img.rockche.cn//image-20210715165036370.png)

- 已设置 xfail_strict = True 时，测试结果显示failed

![image-20210715165109107](https://img.rockche.cn//image-20210715165109107.png)

### addopts

作用：addopts参数可以更改默认命令行选项，这个当我们在cmd输入一堆指令去执行用例的时候，就可以用该参数代替了，省去重复性的敲命令工作

比如：想测试完生成报告，失败重跑两次，一共运行两次，如果在cmd中写的话，命令会很长

```shell
pytest -v --rerun=2 --count=2 --html=report.html --self-contained-html
```

每次都这样敲不太现实，addopts就可以完美解决这个问题

```ini
[pytest]

# 自定义标记说明
markers =
    tencent: tencent
    toutiao: toutiao
    alibaba: alibaba

xfail_strict = True

# 命令行参数
addopts = -v --reruns=1 --count=2 --html=reports.html --self-contained-html
```

### log_cli

作用：控制台实时输出日志

格式：log_cli=True 或False（默认），或者log_cli=1 或 0

#### log_cli=0运行结果

![image-20210715165838558](https://img.rockche.cn//image-20210715165838558.png)

#### log_cli=1运行结果

![image-20210715165852877](https://img.rockche.cn//image-20210715165852877.png)

加了log_cli=1之后，可以清晰看到哪个package下的哪个module下的哪个测试用例是否passed还是failed

### norecursedirs

作用：pytest 收集测试用例时，会递归遍历所有子目录，包括某些你明知道没必要遍历的目录，遇到这种情况，可以使用 norecursedirs 参数简化 pytest 的搜索工作

默认设置： norecursedirs = .* build dist CVS _darcs {arch} *.egg 

正确写法：多个路径用空格隔开

举个🌰，想要不遍历 venv、src、resources、log、report、util文件夹，可以进行如下设置

```ini
[pytest]

norecursedirs = .* build dist CVS _darcs {arch} *.egg venv src resources log report util
```

### 更改测试用例收集规则

pytest默认的测试用例收集规则

> 文件名以 test_*.py 文件和 *_test.py
>
> 以 test_ 开头的函数
>
> 以 Test 开头的类，不能包含 __init__ 方法
>
> 以 test_ 开头的类里面的方法

 我们是可以修改或者添加这个用例收集规则的（建议在原有的规则上添加），如下配置

```ini
[pytest]

python_files =     test_*  *_test  test*
python_classes =   Test*   test*
python_functions = test_*  test*
```

## 注意事项

pytest.ini文件应该放在项目根目录下，不要修改名字，否则无法被识别

## 整理参考

[小菠萝测试笔记](https://www.cnblogs.com/poloyy/p/12702294.html)