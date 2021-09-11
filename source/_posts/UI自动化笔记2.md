---
title: UI自动化学习笔记（二）
date: 2021-04-31  23:12
tags: UI自动化
categories: 自动化测试
---



# 环境搭建

## 安装selenium

- 安装 pip install selenium
- 查看 pip show selenium
- 卸载 pip uninstall selenium
- 指定版本安装 pip install selenium==版本号
- 推荐使用python3.5以上版本

## 安装浏览器驱动

- 火狐
  - https://github.com/mozilla/geckodriver/releases/
- Chrome
  - http://chromedriver.storage.googleapis.com/index.html
- IE
  - http://selenium-release.storage.googleapis.com/index.html

# 第一个案例

```python
from selenium import webdriver
from time import sleep
# 获取浏览器驱动
driver = webdriver.Chorme()
# 打开url
dirver.get("harp://www.baidu.com")
# 暂停3秒
sleep（3）
# 关闭浏览器
driver.quit()
```

# 8种元素定位方式

- id
- name
- class_name（使用元素的class属性定位）
- tag_name（标签名称<标签名.../>）
- link_text(定位超链接a标签)
- partial_link_text(定位超链接 a标签 模糊)
- Xpath（基于元素定位）
- CSS（元素选择器）

汇总

1. 基于元素属性特有定位方式(id\name\class_name)
2. 基于元素标签名称定位：tag_name
3. 定位超链接文本(link_text、partial_link_text)
4. 基于元素路径定位(xpath)
5. 基于选择器(css)

## id定位

### 说明

通过元素的id属性定位，id一般情况下在当前页面中是唯一。

### 方法 

```python
driver.find_element_by_id(id)
```

### 提示

元素必须要有id属性

## name定位

### 说明

通过元素的name属性来定位， name一般名称为重复。

### 方法 

```python
drivr.find_element_by_name(name）
```

### 提示

元素须要有name属性 

## class_name定位

### 说明 

通过元素的class属性来定位，class属性一般为多个值。

### 方法 

```python
driver.find_element_by_class_name()
```

### 提示 

元素必须要有class属性 

## tag_name定位

### 说明

是通过元素的标签名称来定位，标签名(查看元素时尖括号(<)紧挨着的单词或字母就是标签名)(标签名也就是元素名)

### 方法

```python
driver.find_element_by_tag_name("标签名")
```

### 注意

如果页面中存在多个相同标签，默认返回第一个标签元素。

## link_text定位 

### 说明

定位超链接标签

### 方法

```python
driver.find_element_by_link_text()
```

### 注意

link_text:只能使用精准匹配(a标签的全部文本内容)

## partial_link_text【推荐】

### 说明

定位超链接标签

### 方法

```python
driver.find_element_by_partial_link_text()
```

### 注意： 

1. 可以使用精准或模糊匹配，如果使用模糊匹配最好使用能代表唯一的关键词
2. 如果有多个值，默认返回第一个值

## Xpath定位

### Xpath常用的定位策略

1. 路径
   1). 绝对路径：
   语法：以单斜杠开头逐级开始编写，不能跳级。如：/html/body/div/p[1]/input
   2). 相对路径
   语法：以双斜杠开头，双斜杠后边跟元素名称，不知元素名称可以使用*代替。 如： //input //*

2. 路径结合属性
   语法：在Xpath中，所有的属性必须使用@符号修饰 如：//*[@id='id值']

3. 路径结合逻辑(多个属性)
   语法：//*[@id="id值" and @属性='属性值']

4. 路径结合层级
   语法：//*[@id='父级id属性值']/input
   			

   	提示：
   		1. 一般见识使用指定标签名称，不使用*代替，效率比较慢。
   		2. 无论是绝对路径和相对路径，/后面必须为元素的名称或者*
   		3. 扩展：在工作中，如果能使用相对路径绝对不使用绝对路径。

### Xpath扩展

		1. //*[text()='XXX'] # 定位文本值等于XXX的元素  
			提示：一般适合 p标签，a标签 
		2. //*[contains(@属性,'xxx')] # 定位属性包含xxx的元素 【重点】
			提示：contains为关键字，不可更改。 
		3. //*[starts-with(@属性,'xxx')] # 定位属性以xxx开头的元素
			提示：starts-with为关键字不可更改	

## CSS定位

### 说明

1. CSS一种标记语言，焦点：数据的样式。控制元素的显示样式，就必须先找到元素，在css标记语言中找元素使用css选择器；
2. css定位就是通过css选择器工具进行定位。
3. 极力推荐使用，查找元素的效率比xpath高，语法比xpath更简单。

### 方法

```python
driver.find_element_by_css_selector()
```

### 常用测试略：

1. id 选择器
   前提：元素是必须有id属性
   语法：#id  如：#passwordA
2. class 选择器
   前提：元素是必须有class属性
   语法：.class  如：.telA
3. 元素选择器
   语法：element  如：input
4. 属性选择器
   语法：[属性名=属性值]
5. 层级选择器
   语法： 
6. p>input 
7. p input 
   提示：>与空格的区别，大于号必须为子元素，空格则不用。

### 扩展： 

1. [属性^='开头的字母'] # 获取指定属性以指定字母开头的元素
2. [属性$='结束的字母'] # 获取指定属性以指定字母结束的元素
3. [属性*='包含的字母'] # 获取指定属性包含指定字母的元素
   	

复制xpath:/html/body/form/div/fieldset/p[1]/input
复制最简：//*[@id="userA"]
复制CSS路径：html body form div#zc fieldset p#p1 input#userA
		
提示： 

1. 虽然借助工具可以快速生成xpath路径和css语法，但是前期不建议使用。
2. 工具再智能，没有人智能。

## 定位一组元素

### 方法

```python
driver.find_elements_by_xxx()
```

返回结果：类型为列表，要对列表进行访问和操作必须指定下标或进行遍历，[下标从0开始]

## 扩展8种元素定位的底层实现

### 方式

```python
driver.find_element(By.xxx, 'value')
```

### 参数说明

By.xxx :为By类的类型 如：By.ID
value: 元素的定位值 如： "userA"
By类：需要导包 位置： from selenium.webdriver.common.by import By

# 元素操作

## 方法

- send_keys() # 输入方法
- click() # 点击方法
- clear() # 清空

# 浏览器常用操作API

## 方法

- driver.maximize_window() # 最大化浏览器

- driver.set_window_size(w, h) # 设置浏览器大小 单位像素

- driver.set_window_position(x, y) # 设置浏览器位置

- driver.back() # 后退操作

- driver.forward() # 前进操作

- driver.refresh() # 刷新操作

- driver.close() # 关闭当前主窗口(主窗口：默认启动哪个界面，就是主窗口)

- driver.quit() # 关闭由driver对象启动的所有窗口

- driver.title # 获取当前页面title信息

- drive.current_url # 获取当前页面url信息

  

## 提示

- driver.title 和 driver.current_url 没有括号，应用场景：一般为判断上步操作是否执行成功。
 <!-- more -->

- driver.maximize_window() # 一般为我的前置代码，在获取driver后，直接编写最大化浏览器
- driver.refresh() 应用场景，在后面的cookie章节会使用到。
- driver.close()与driver.quit()区别：
  - 	close():关闭当前主窗口
  - 	quit():关闭由driver对象启动的所有窗口
- 提示：如果当前只有1个窗口，close与quit没有任何区别。

# 元素信息操作API

## 方法

- text 获取元素文本  如：driver.text
- size 获取元素大小  如：driver.size 
- get_attribute 获取元素属性值 如：driver.get_attribute("id")
- is_displayed 判断元素是否可见 如：element.is_displayed()
- is_enabled 判断元素是否可用 如: element.is_enabled()
- is_selected 判断元素是否被选中 如：element.is_selected()		

## 提示 

- text和size调用时 无括号
- get_attribute一般应用场景：判断一组元素是否为想要的元素或者判断元素属性值是否
- is_displayed、is_enabled、is_selected，在特殊应用场景中使用。

# 鼠标操作

## 为什么使用鼠标操作

为了满足丰富的html鼠标效果，必须使用对于的方法

## 鼠标事件对应的方法在哪个类中

from selenium.webdirver.common.action_chains import ActionChains

## 鼠标时间常用的方法

- context_click() #右击
  - 应用：context_click(element).perform()
- double_click() #双击
  - 应用：double_click(element).perform()
- drag_and_drop() # 拖拽
  - 应用：drag_and_drop(source, target).perform
- move_to_element() #悬停
- move_to_element(element).perform()
- perform() # 执行以上事件方法

# 键盘操作

## 键盘对应的方法在Keys类中

包：from selenium.webdriver.common.keys import Keys

## 常用的快捷键：

CONTROL：Ctrl键
其他，请参考Keys底层定义的常量

## 应用

组合键：element.send_keys(Keys.XXX, 'a')
单键：element.send_keys(Keys.XXX)

# 元素等待

## 为什么要设置元素等待

由于电脑配置或网络原因，在查找元素时，元素代码未在第一时间内被加载出来，而抛出未找到元素异常。

## 什么是元素等待

元素在第一次未找到时，元素等待设置的时长被激活，如果在设置的有效时长内找到元素，继续执行代码，如果超出设置的时长未找打元素，抛出未找到元素异常。

## 元素等待分类

- 隐式等待

  - 方法：driver.implicitly_wait(30) # 一般情况下设置30秒
    特色：
    - 针对所有元素生效
    - 一般情况下为前置必写代码(1.获取浏览器驱动对象；2. 最大化浏览器；3. 设置隐式等待)

- 显示等待

  - 导包

    - ```python
      from selenium.webdirver.support.wait import WebDriverWait
      ```

  - WebDriverWait(driver,timout,poll_frequency=0.5)

    - driver: 浏览器驱动对象
    - timeout：超时的时长。单位：秒
    - poll_frequency：检测间隔时间，默认0.5秒

  - 调用方法 untili(method): 直到...时

    - method：函数名称，该函数用来实现对元素的定位
    - 一般使用匿名函数：lambda x:x.find_element_by_id("xxxxx")

-   显示等待与隐式等待区别：
    - 显示等待：针对单个元素生效
    - 隐式等待：针对全局元素生效