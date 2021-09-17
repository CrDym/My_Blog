---
title: Selenium系列4-元素定位
date: 2021-09-14  22:28
tags: UI自动化
categories: 自动化测试
---

## 前言

说起元素定位，一定是学习自动化测试绕不开的第一道关，无论是web端的UI自动化还是移动端的自动化，在需要首先对元素进行定位才可以完成对元素的操作已达成测试目的，在Selenium中，可以使用`find_element`（定位单个元素）或`find_elements`（定位多个元素）方法来定位元素。

## Selenium元素定位常用API

在工作中我们常用的元素定位API一共有8种，我们先来了解以下6种，xpath和css_selector我们在后面的文章中单独学习

### 通过id定位

**说明**

当所定位的元素具有`id`属性时，我们可以使用by_id来定位该元素，id一般情况下在当前页面中是唯一的。

**语法**

```python
drivr.find_element_by_id(id）
```

**示例**

打开百度首页，定位搜索框，查看页面元素，可以看到搜索框元素的id为 `kw`

![image-20210915151758333](https://img.rockche.cn//image-20210915151758333.png)

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 元素定位
el = driver.find_element_by_id('kw')
# 打印元素
print(el)
# 查看元素对应的源码
print(el.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210915154738133](https://img.rockche.cn//image-20210915154738133.png)

可以看到el是一个WebElement类型的对象

**定位多个元素**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 元素定位
els = driver.find_elements_by_id('kw')

# 查看返回结果的数据类型
print("数据类型", type(els))
print("元素个数", len(els))

# 遍历结果，查看元素源码
for i in els:
    print(i.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210915161212582](https://img.rockche.cn//image-20210915161212582.png)

可以看到返回数据的类型为list，元素个数为1个

### 通过name定位

**说明**

当所定位的元素具有`id`属性时，我们可以使用by_name来定位该元素，name一般情况下在当前页面中不是唯一的。

**语法**

```python
drivr.find_element_by_name(name）
```

**示例**

打开百度首页，定位搜索框，查看页面元素，可以看到搜索框元素的name为 `wd`

![image-20210915162228193](https://img.rockche.cn//image-20210915162228193.png)

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 元素定位
el = driver.find_element_by_name('wd')
# 打印元素
print(el)
# 查看元素对应的源码
print(el.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210915154738133](https://img.rockche.cn//image-20210915154738133.png)

可以看到el是一个WebElement类型的对象

**定位多个元素**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 元素定位
els = driver.find_elements_by_name('wd')

# 查看返回结果的数据类型
print("数据类型", type(els))
print("元素个数", len(els))

# 遍历结果，查看元素源码
for i in els:
    print(i.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210915161212582](https://img.rockche.cn//image-20210915161212582.png)

可以看到返回数据的类型为list，元素个数为1个

### 通过class_name定位

**说明**

当所定位的元素具有`class`属性时，我们可以使用by_class_name来定位该元素，class属性一般为多个值。

**语法**

```python
drivr.find_element_by_class_name(class属性值）
```

**示例**

打开百度首页，定位搜索框，查看页面元素，可以看到搜索框元素的class_name为 `s_ipt`

![image-20210916152122253](https://img.rockche.cn//image-20210916152122253.png)

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 元素定位
el = driver.find_element_by_class_name('s_ipt')
# 打印元素
print(el)
# 查看元素对应的源码
print(el.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210915154738133](https://img.rockche.cn//image-20210915154738133.png)

可以看到el是一个WebElement类型的对象

**定位多个元素**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 元素定位
els = driver.find_elements_by_class_name('s_ipt')

# 查看返回结果的数据类型
print("数据类型", type(els))
print("元素个数", len(els))

# 遍历结果，查看元素源码
for i in els:
    print(i.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210915161212582](https://img.rockche.cn//image-20210915161212582.png)

可以看到返回数据的类型为list，元素个数为1个

### 通过tag_name定位

**说明**

通过元素的标签名称来定位，如果页面中存在多个相同标签，默认返回第一个标签元素

**语法**

```python
drivr.find_element_by_tag_name("标签名"）
```

**示例**

打开网易企业邮箱登录界面，定位登录按钮，查看页面元素，可以看到登录按钮的tag_name为 `button`

![image-20210916154544887](https://img.rockche.cn//image-20210916154544887.png)

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://qiye.163.com/login/"
driver.get(url)
sleep(2)

# 元素定位
el = driver.find_element_by_tag_name('button')
# 打印元素
print(el)
# 查看元素对应的源码
print(el.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210916155003704](https://img.rockche.cn//image-20210916155003704.png)

可以看到el是一个WebElement类型的对象

**定位多个元素**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://qiye.163.com/login/"
driver.get(url)
sleep(2)

# 元素定位
els = driver.find_elements_by_tag_name('button')

# 查看返回结果的数据类型
print("数据类型", type(els))
print("元素个数", len(els))

# 遍历结果，查看元素源码
for i in els:
    print(i.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210916155052862](https://img.rockche.cn//image-20210916155052862.png)

可以看到返回数据的类型为list，元素个数为2个

### 通过link_text定位

**说明**

`by_link_text`通过超文本链接上的文字信息来定位元素，一般专门用于定位页面上的超文本链接。

**语法**

```python
drivr.find_element_by_link_text("全部文本"）
```

**示例**

打开百度首页，定位点击超链接 新闻

![image-20210916162710938](https://img.rockche.cn//image-20210916162710938.png)

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 元素定位
el = driver.find_element_by_link_text('新闻')
# 打印元素
print(el)
# 查看元素对应的源码
print(el.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210916163157767](https://img.rockche.cn//image-20210916163157767.png)

可以看到el是一个WebElement类型的对象

**定位多个元素**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 元素定位
els = driver.find_elements_by_link_text('新闻')

# 查看返回结果的数据类型
print("数据类型", type(els))
print("元素个数", len(els))

# 遍历结果，查看元素源码
for i in els:
    print(i.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210916163131536](https://img.rockche.cn//image-20210916163131536.png)

可以看到返回数据的类型为list，元素个数为1个

### 通过partial_link_text定位

**说明**

当不确定超链接上的文本信息或者只想通过一些关键字进行匹配时，可以使用`by_partial_link_text`这个方法来通过部分链接文字进行匹配

> 可以使用精准或模糊匹配，如果使用模糊匹配最好使用能代表唯一的关键词
>
> 如果有多个值，默认返回第一个值

**语法**

```python
drivr.find_element_by_partial_link_text("部分文本"）
```

**示例**

打开百度首页，定位点击超链接 hao123

![image-20210916163652350](https://img.rockche.cn//image-20210916163652350.png)

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 元素定位
el = driver.find_element_by_partial_link_text('hao')
# 打印元素
print(el)
# 查看元素对应的源码
print(el.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210916164000370](https://img.rockche.cn//image-20210916164000370.png)

可以看到el是一个WebElement类型的对象

**定位多个元素**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 实例化浏览器对象
driver = webdriver.Chrome()

# 访问被测网址
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 元素定位
els = driver.find_element_by_partial_link_text('hao')

# 查看返回结果的数据类型
print("数据类型", type(els))
print("元素个数", len(els))

# 遍历结果，查看元素源码
for i in els:
    print(i.get_attribute('outerHTML'))
# 关闭浏览器
driver.quit()
```

输出结果如下：

![image-20210916164020208](https://img.rockche.cn//image-20210916164020208.png)

可以看到返回数据的类型为list，元素个数为1个

## 参考

https://www.cnblogs.com/liuyuelinfighting/p/14925556.html

