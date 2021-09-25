---
title: Selenium系列6-使用XPath定位元素
date: 2021-09-22  22:28
tags: UI自动化
categories: 自动化测试
---

## 前言

我们在进行自动化测试时，常会遇到素没有`id`，`name`，`class`属性，或者`id`，`name`，`class`属性是动态的，这时该如何定位呢？

通常会使用`XPath`和`css_selector`，这两种方式可以解决90%的元素定位



## 使用Xpath定位

XPath定位和Selenium基础元素定位方式一样，既可以获取单个元素，也可以获取多个元素集

获取一个元素对象：
`driver.find_element_by_xpath("XPath路径表达式")`

获取多个元素集：
`driver.find_elements_by_xpath("XPath路径表达式")`



### Xpath通过id，name，class属性定位

Xpath表达式：//标签名[@属性名='属性值']

**示例**

在百度首页中，使用Xpath定位搜索框

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 打开浏览器
driver = webdriver.Chrome()

# 打开被测地址
url = "https://wwww.baidu.com"
driver.get(url)

# 用XPath通过id属性定位
el1 = driver.find_element_by_xpath("//input[@id='kw']")
print(el1.get_attribute("outerHTML"))

# 用XPath通过name属性定位
el2 = driver.find_element_by_xpath("//input[@name='wd']")
print(el2.get_attribute("outerHTML"))

# 用XPath通过class属性定位
el3 = driver.find_element_by_xpath("//*[@class='s_ipt']")
print(el3.get_attribute("outerHTML"))

# 关闭浏览器
driver.quit()
```

**输出结果**

![image-20210923144812099](https://img.rockche.cn//image-20210923144812099.png)

> - 不指定标签名称时，使用*号表示任意标签
> - 指定标签名称时，使用对应标签名称



### Xpath通过标签中的其他属性定位

如果元素没有id、name、class属性，可以通过其他属性进行定位

**示例**

使用Xpath通过其他属性对百度搜索框进行定位

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 打开浏览器（获得浏览器对象）
driver = webdriver.Chrome()

# 打开页面
url = "http://www.baidu.com"
driver.get(url)
sleep(2)

# 用XPath通过maxlength属性定位  
el1 = driver.find_element_by_xpath("//input[@maxlength='255']")
# 打印定位元素所在行的源码
print(el1.get_attribute("outerHTML"))

# 5.用XPath通过autocomplete属性定位  
el2 = driver.find_element_by_xpath("//input[@autocomplete='off']")
print(el2.get_attribute("outerHTML"))

# 6.关闭浏览器
driver.quit()
```

**输出结果**

![image-20210924114737921](https://img.rockche.cn//image-20210924114737921.png)



### Xpath层级定位

如果元素的属性无法直接定位到，我们可以通过它的父元素然后通过层级关系定位

**示例**

使用Xpath通过层级定位对百度首页搜索框

![image-20210924144310196](https://img.rockche.cn//image-20210924144310196.png)

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 打开浏览器（获得浏览器对象）
driver = webdriver.Chrome()

# 打开页面
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 用XPath通过层级定位
el = driver.find_element_by_xpath("//form[@id='form']/span/input")
print(el.get_attribute("outerHTML"))

# 6.关闭浏览器
driver.quit()
```

**输出结果**

![image-20210924144342148](https://img.rockche.cn//image-20210924144342148.png)



### Xpath索引定位

如果一个元素的标签和他兄弟元素的标签一样，这时候无法通过层级定位的。因为都是一个父亲生的，多胞胎兄弟。

虽然双胞胎兄弟很难识别，但是出生是有先后的，于是可以通过它在家里的排行老几定位到（这里索引是从1开始算起的，跟Python的索引不一样）

**示例**

使用xpath索引定位百度首页左上角的按钮

![image-20210924150500970](https://img.rockche.cn//image-20210924150500970.png)

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 打开浏览器（获得浏览器对象）
driver = webdriver.Chrome()

# 打开页面
url = "https://www.baidu.com"
driver.get(url)
sleep(2)

# 用XPath通过索引定位 新闻按钮
el1 = driver.find_element_by_xpath("//div[@id='s-top-left']/a[@target='_blank'][1]")
print(el1.get_attribute("outerHTML"))

# 用XPath通过索引定位 地图按钮
el2 = driver.find_element_by_xpath("//div[@id='s-top-left']/a[@target='_blank'][3]")
print(el2.get_attribute("outerHTML"))

# 用XPath通过索引定位 直播按钮
el3 = driver.find_element_by_xpath("//div[@id='s-top-left']/a[@target='_blank'][4]")
print(el3.get_attribute("outerHTML"))

# 6.关闭浏览器
driver.quit()
```

**输出结果**

![image-20210924150440204](https://img.rockche.cn//image-20210924150440204.png)



### Xpath逻辑定位

当几个元素具有相同的属性和属性值时，就没办法用一个属性来通过Xpath进行定位了，需要使用多个属性进行定位

XPath逻辑定位说明：

- 支持与（`and`）、或（`or`）、非（`not`）

- 一般用的比较多的是`and`运算，同时满足两个属性

**示例**

使用Xpath运算符定位账号输入框

![image-20210924151804220](https://img.rockche.cn//image-20210924151804220.png)

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep

# 打开浏览器（获得浏览器对象）
driver = webdriver.Chrome()

# 打开页面
url = "https://xiaojing0.com/admin/login"
driver.get(url)
sleep(2)

# 用XPath通过运算符定位
el1 = driver.find_element_by_xpath("//input[@type='text' and @placeholder='账号']")
print(el1.get_attribute("outerHTML"))

# 关闭浏览器
driver.quit()
```

**输出结果**

![image-20210924152520372](https://img.rockche.cn//image-20210924152520372.png)



### Xpath模糊匹配定位

有些时候，标签元素的属性值太长，我们可以使用模糊匹配的定位方式来定位元素

**示例**

定位下面代码片段中的指定元素

```html
<bookstore id='zc'>
    <book id='b1'>
        <title lang="eng">Harry Potter</title>
        <price>29.99</price>
    </book>

    <book id='b2'>
        <title lang="eng" id="t2">Learning XML</title>
        <price>39.95</price>
    </book>
    <p>
        <book id='b3'>
            <title lang="eng" id="t3">Learning HTML</title>
            <price>99.95</price>
        </book>
    </p>
</bookstore>

```

> 相对路径：模糊查询
>
> - *：匹配任何元素节点
> - @*：匹配任何属性元素节点
> - node()：匹配任何类型的节点

**代码**

```python
# 导入selenium
from selenium import webdriver
from time import sleep
import os

# 打开浏览器
driver = webdriver.Chrome()

# 打开注册A页面
url = "file:///" + os.path.abspath("./demo1.html")
driver.get(url)
sleep(2)

# XPath模糊定位
# 1 选取 bookstore 元素的所有子元素。
element_1 = driver.find_elements_by_xpath("//bookstore/*")
for element in element_1:
    print(element.get_attribute("outerHTML"))

# 2 匹配绝对路径最外层元素
element_2 = driver.find_element_by_xpath("/*")
print(element_2.get_attribute("outerHTML"))

# 3 选取页面中的所有元素。
element_3 = driver.find_element_by_xpath("//*")
print(element_3.get_attribute("outerHTML"))

# 4 选取所有带有属性的 title 元素。
element_4 = driver.find_element_by_xpath("//title[@*]")
print(element_4.get_attribute("outerHTML"))

# 5 匹配所有有属性的节点（属性模糊查询使用很少）。
element_5 = driver.find_element_by_xpath("//*[@*]")
print(element_5.get_attribute("outerHTML"))

# 6 匹配bookstore节点所有孙子辈的id属性值为t2的title节点
textA = driver.find_element_by_xpath("//bookstore/node()/title[@id='t2']")
print(textA.get_attribute("outerHTML"))

# 关闭浏览器
driver.quit()
```



### Xpath其他定位方式

`contains`关键字，是用于模糊查询定位，意思是属性中含有xxx的元素。

内容匹配可以是部分内容，也可以是全部内容。

**contains（text()，"xx")**

模糊匹配text

**示例**

```python
# 1.导入selenium
from selenium import webdriver
from time import sleep
import os

# 2.打开浏览器
driver = webdriver.Chrome()

# 3.打开页面
url = "https://www.baidu.com"
driver.get(url)

# 4.使用xpath模糊匹配，使用contains
el= driver.find_element_by_xpath("//a[contains(text(), '新闻')]")
print(el.get_attribute("outerHTML"))

# 5.关闭浏览器
sleep(2)
driver.quit()
```



**contains（@属性名，"属性值")**

模糊匹配某个属性

**示例**

```python
# 1.导入selenium
from selenium import webdriver
from time import sleep
import os

# 2.打开浏览器
driver = webdriver.Chrome()

# 3.打开页面
url = "https://www.baidu.com"
driver.get(url)

# 4.使用xpath模糊匹配，使用contains
el= driver.find_element_by_xpath("//input[contains(@class,'s_ip')]")
print(el.get_attribute("outerHTML"))

# 5.关闭浏览器
sleep(2)
driver.quit()
```



**starts-with**

模糊匹配以xx开头

**示例**

```python
# 1.导入selenium
from selenium import webdriver
from time import sleep
import os

# 2.打开浏览器
driver = webdriver.Chrome()

# 3.打开页面
url = "https://www.baidu.com"
driver.get(url)

# 4.使用xpath模糊匹配，使用starts-with
el = driver.find_element_by_xpath("//input[starts-with(@class,'s_ip')]")
print(el.get_attribute("outerHTML"))

# 5.关闭浏览器
sleep(2)
driver.quit()
```



**text()**

用于纯文字的查找

**示例**

```python
# 1.导入selenium
from selenium import webdriver
from time import sleep
import os

# 2.打开浏览器
driver = webdriver.Chrome()

# 3.打开页面
url = "https://www.baidu.com"
driver.get(url)

# 4.使用xpath模糊匹配，使用starts-with
el = driver.find_element_by_xpath("//*[text()='hao123']")
print(el.get_attribute("outerHTML"))

# 5.关闭浏览器
sleep(2)
driver.quit()
```



## 参考

https://www.cnblogs.com/liuyuelinfighting/p/14943233.html