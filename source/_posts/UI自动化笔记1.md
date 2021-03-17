---
title: UI自动化学习笔记（一）
date: 2021-03-31  22:28
tags: UI自动化
categories: 自动化测试
---

# 主流的自动化测试工具

- QTP
  - QTP是一个商业的自动化测试工具，收费，支持web、桌面自动化测试
- Selenium
  - Selenium是一个开源的web自动化测试工具，免费
- Robot framework
  - Robot framework是一个基于Python可扩展的关键字驱动的测试自动化框架（基本上已被淘汰）
- Airtest
  - Airtest是网易公司的基于图像识别的UI自动化测试工具，多用于测试手机游戏，也可以用于APP、小程序测试

# Selenium特点

- 开源
- 跨平台：Linux、windows、Mac
- 支持多种浏览器：Firefox、Chrome、IE、Edge、Opera、Safari等
- 支持多种语言：java、python、C#、Javascript、Ruby、Php等
- 成熟稳定
- 功能强大
<!-- more -->

# Selenium的发展

- Selenium1.0
  - seleniumIDE      
  - seleniumGrid       
  - seleniumRC
- Selenium2.0    
  - Selenium1.0+WebDriver
- Selenium3.0
  - 弃掉了seleniumRC
  - 全面拥抱java8
  - 支持MacOS
  - 通过Mozilla官方的geckodriver来支持firefox

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
1. p>input 
2. p input 
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