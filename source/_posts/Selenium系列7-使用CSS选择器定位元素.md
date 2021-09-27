---
title: Selenium系列7-使用CSS选择器定位元素
date: 2021-09-27  22:28
tags: UI自动化
categories: 自动化测试
---

## CSS选择器介绍

CSS（Cascading Style Sheets）是一种语言，它被用来描述HTML 和XML 文档的表现。CSS 使用选择器来为页面元素绑定CSS属性。这些选择器可以被Selenium 用作另外的定位策略。

by_css_selector通过CSS选择器查找元素，这种元素定位方式跟by_xpath比较类似，Selenium官网的Document里极力推荐使用CSS locator，而不是XPath来定位元素。原因是CSS locator比XPath locator速度快，特别是在IE下，CSS locator比XPath更高效更准确更易编写，对各种浏览器支持也很好。

## Selenium中使用CSS选择器定位元素

### 通过属性定位元素

css选择器可以通过元素的id、class、标签这三个常规属性定位元素

**示例**

使用css选择器通过元素的id、class、标签定位百度搜索框

![image-20210927225641578](https://img.rockche.cn//image-20210927225641578.png)

**语法**

> - 通过id定位：find_element_by_css_selector("#kw")   
>
> - 通过class定位：find_element_by_css_selector(".s_ipt")   . 表示通过class定位
>
> - 通过标签名定位：find_element_by_css_selector("标签名“)  

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

# 用css通过id定位
el = driver.find_element_by_css_selector("#kw")
print(el.get_attribute("outerHTML"))

# 用css通过class定位
el2 = driver.find_element_by_css_selector(".s_ipt")
print(el2.get_attribute("outerHTML"))

# 用css通过标签定位（存在多个）
el3 = driver.find_elements_by_css_selector("input")
# 打印定位到的元素个数
print(len(el3))

# 关闭浏览器
driver.quit()
```

**输出结果**

![image-20210927225555198](https://img.rockche.cn//image-20210927225555198.png)



### 通过标签+属性定位元素

通过 标签+属性 即 标签名[属性名=属性值]的方式来定位

**示例**

还是以百度搜索框为例

**语法**

> find_element_by_css_selector("input[autocomplete='off']")

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

# 用css通过标签+属性定位
el = driver.find_element_by_css_selector("input[autocomplete='off']")
print(el.get_attribute("outerHTML"))


# 关闭浏览器
driver.quit()
```

**输出结果**

![image-20210927230247861](https://img.rockche.cn//image-20210927230247861.png)

**注意**

> 和xpath定位不同的是，标签名前不用// ，[ ]内的属性名前不用@符号，而xpath则需要。其余的规则与xpath相同。 
> 如果属性是唯一的，那么标签名可以不用写。



### 通过层级关系定位元素

通过 父标签[父标签属性名=父标签属性值]>（或者空格）子标签  

**示例**

以百度搜索框按钮为例，通过父标签中的class属性定位，然后找到子标签input

**语法**

> find_element_by_css_selector("span[class='bg s_btn_wr']>input")

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

# 用css通过标签+属性定位
el = driver.find_element_by_css_selector("span[class='bg s_btn_wr']>input")
print(el.get_attribute("outerHTML"))


# 关闭浏览器
driver.quit()
```

**输出结果**

![image-20210927231145623](https://img.rockche.cn//image-20210927231145623.png)



### 通过索引定位元素

当父标签中有很多相同的子标签时，通过索引找到所需要定位的元素

通过 父标签[父标签属性名=父标签属性值]>子标签：nth-child(索引序号) 

**示例**

以xiaojing0的登录按钮为例

![image-20210927234120686](https://img.rockche.cn//image-20210927234120686.png)

**语法**

> find_element_by_css_selector（"form[class='el-form page-signin__form']>div:nth-child(3)"）
>
> 用css_selector进行元素定位，父标签到子标签都用>或空格。如果用的是>，意思是指第一个子标签
> 而用空格的话，则可以为任何子标签

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
sleep(3)

# 使用css选择器通过索引定位
el = driver.find_element_by_css_selector("form[class='el-form page-signin__form']>div:nth-child(3)")
print(el.get_attribute("outerHTML"))


# 关闭浏览器
driver.quit()
```

**输出结果**

![image-20210927234158669](https://img.rockche.cn//image-20210927234158669.png)



### 通过逻辑运算定位元素

css选择器可以实现逻辑运算，同时匹配两个属性

标签名[属性名1= 属性值1] [属性名2=属性值2]

**示例**

以xiaojing0的账号输入框为例

![image-20210927235440384](https://img.rockche.cn//image-20210927235440384.png)

**语法**

> `find_element_by_css_selector（"input[type='text'][placeholder='账号']"）`

**代码**

```
# 导入selenium
from selenium import webdriver
from time import sleep

# 打开浏览器（获得浏览器对象）
driver = webdriver.Chrome()

# 打开页面
url = "https://xiaojing0.com/admin/login"
driver.get(url)
sleep(3)

# 使用css选择器通过索引定位
el = driver.find_element_by_css_selector("input[type='text'][placeholder='账号']")
print(el.get_attribute("outerHTML"))


# 关闭浏览器
driver.quit()
```

**输出结果**

![image-20210927235333426](https://img.rockche.cn//image-20210927235333426.png)



### 通过模糊匹配定位元素

css选择器有三种模糊匹配方式

- ^：以什么开头
- $：以什么结尾
- *：匹配所有

**语法**

>  find_element_by_css_selector（“标签名[属性名*（或^，或$）='属性值']”）



## 参考

https://www.cnblogs.com/liuyuelinfighting/p/14950596.html

https://www.cnblogs.com/leolsl/p/13084523.html