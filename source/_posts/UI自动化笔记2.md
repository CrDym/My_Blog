---
title: UI自动化学习笔记（二）
date: 2021-04-31  23:12
tags: UI自动化
categories: 自动化测试
---

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