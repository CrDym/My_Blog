---
title: Selenium系列3-浏览器对象操作
date: 2021-09-13  22:28
tags: UI自动化
categories: 自动化测试
---

## 前言

在我们日常进行UI自动化工作时，对浏览器对象的操作是所有用例的基础，而selenium的API，可以分为三大类：

- 对浏览器本身的相关操作
- 对浏览器页面中元素的定位
- 对定位后元素的操作（如点击、输入等）

所以在深入了解selenium前，我们先来看看selenium中是如何操作浏览器本身的

## 导入selenium库

要使用selenium，必须先导入Selenium库

```python
# 导入selenium库
from selenium import webdriver
```

## 创建浏览器对象

创建一个浏览器对象

```python
# 语法：driver = webdriver.xxx()     xxx为对应浏览器
driver = webdriver.Chrome()
```

## 浏览器窗口大小设置

我们在测试过程中，经常会有改变浏览器大小的场景，可以使用`set_window_size()`方法改变浏览器的大小

如果需要获取浏览器当前的大小数值，可以使用`get_window_size()`	

也可以使用`maximize_window()`将浏览器最大化

```python
# 设置浏览器尺寸
# 宽600、高1000
driver.set_window_size(600，1000）

# 获取浏览器尺寸
driver.get_window_size()		

# 浏览器窗口最大化（常用）
driver.maximize_window()
```

## 浏览器位置设置

那么如果需要移动浏览器的位置改如何实现呢，selenium提供了`set_window_position()`方法改变浏览器的位置

如果需要获取浏览器当前的位置，可以使用`get_window_position()`	

> 显示器以左上角为`(0,0)`，所有的位置操作都是相对于显示器左上角展开的位移操作

```python
# 获取浏览器的当前位置
driver.get_window_position()		

# 设置浏览器的位置
driver.set_window_position(x,y)
```

## 访问被测网址

```python
# 请求被测网址
# 语法：driver.get(url)	
url = "http://www.baidu.com"
driver.get(url)
```

## 浏览器页面前进、后退和刷新

```python
# 页面前进
driver.forward()

# 页面后退
driver.back()

# 页面刷新
driver.refresh()
```

## 关闭浏览器

在测试用例执行结束之后，我们需要关闭浏览器，selenium提供了两种关闭浏览器的方法

- 关闭当前窗口

```python
# 关闭当前浏览器窗口
driver.close()
```

- 关闭窗口并关闭浏览器驱动

```python
# 即关闭浏览器窗口，同时关闭浏览器驱动
driver.quit()
```

## 示例

通过以下的例子来看看这些方法的实际使用

```python
# 导入selenium库
from time import sleep
from selenium import webdriver

# 创建浏览器对象
driver = webdriver.Chrome()

# 设置浏览器窗口大小
driver.set_window_size(480, 800)
# 获取当前浏览器窗口大小
s = driver.get_window_size()
print(s)
# 最大化浏览器窗口
driver.maximize_window()
sleep(3)

# 获取当前浏览器位置
p1 = driver.get_window_position()
print(p1)
# 移动浏览器位置
driver.set_window_position(400, 300)
p2 = driver.get_window_position()
print(p2)
sleep(3)

# 访问被测地址
driver.get("https://www.baidu.com")
sleep(2)
driver.get("https://www.jd.com")
sleep(2)
driver.get("https://www.taobao.com")
sleep(2)
# 使用前进，后退，刷新命令
# 前进
driver.back()  # 后退到京东
sleep(2)
driver.back()  # 后退到百度
sleep(2)
# 后退
driver.forward()  # 前进到京东
sleep(2)
driver.forward()  # 前进到淘宝
sleep(2)
# 刷新
driver.refresh()  # 保持在淘宝页面
sleep(2)
# 关闭浏览器
driver.quit()
```

## 参考

https://www.cnblogs.com/liuyuelinfighting/p/14923204.html
