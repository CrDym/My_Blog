---
title: UI自动化学习笔记（三）
date: 2021-05-12  16:12
tags: UI自动化
categories: 自动化测试
---
# 下拉选择框

- 为什么单独使用下拉框？
  - 如果option选项没有value值的化，css定位或其他定位就不太方便。
- 如何使用Select类
  - 操作：
    - 导包：from  selenium.webdriver.support.select improt Select
    -  实例化：s = Select(element)
    - 调用方法：s.select_by_index()
- 提供哪些方法
  - select_by_index() # 通过下标定位
  - select_by_value() # 通过value值
  - select_by_visible_text() #显示文本
- 注意事项
  - 实例化select时，需要的参数为 select标签元素
  - 调用Select类下面的方法，是通过索引、value属性值、显示文本去控制，而不需要click事件
   <!-- more -->

# 对话框处理

## 为什么要处理对话框

如果页面有弹出框，不做处理，接下来的元素获取将不生效。

## 对话框类型

- alert # 警告框
- confirm # 确认框
- prompt # 提示框

## 如何处理

以上三种对话框，处理方法都一样。
步骤：

- 切换到对话框
  - 方法：driver.switch_to.alert
- 处理对话框
  - alert.text # 获取文本
  - alert.accept() # 同意
  - alert.dismiss() # 取消
- 提示
  - 无论以上哪个对话框，都可以使用取消、同意，因为调用的是后台的事件，根页面显示的按钮数量无关

## 注意

- driver.switch_to.alert 方法适合以上三种类型对话框，调用时没有括号
- 获取文本的方法，调用时没有括号 如：alert.text
- 在项目中不是所有的小窗口都是以上三种对话框。



# 滚动条处理

## 为什么要操作滚动条

在web自动化中有些特殊场景，如：滚动条拉倒最底层，指定按钮才可用。

- 如何操作 

  - 设置操作滚动条操作语句

    如：js = "window.scrollTo(0,10000)"

    - 0: 左边距 -->水平滚动条
    - 10000：上边距 -->垂直滚动条

  - 调用执行js方法，将设置js语句传入方法中

    - 方法：driver.execute_script(js)

- 说明

  - 在selenium中没有直接提供定位滚动条组件方法，但是它提供了执行js语句方法，可以通过js语句来控制滚动条操作。

# 切换frame表单

常用的frame表单有两种：frame、iframe

## 为什么要切换

当前目录内没有iframe表单页面元素信息，不切换的话无法找到元素

## 如何切换

方法：driver.switch_to.frame("id\name\element")

## 为什么切换完之后要回到主目录

ifame或frame只有在主目录才有相关元素信息，不会到主目录，切换语句会报错

## 如何回到主目录

方法：driver.switch_to.default_content()

## 提示

切换frame时，可以使用name、id、iframe元素



# 多窗口切换

## 为什么要切换多窗口

页面存在多个窗口时，selenium默认焦点只会在主窗口上的所有元素，不切换窗口，无法操作主窗口以外的窗口内元素

## 如何切换

- 思路
  - 获取要切换的窗口句柄，调用切换方法进行切换。

- 方法 
  - driver.current_window_handle # 获取当前主窗口句柄
  -  driver.window_handles # 获取当前由driver启动所有窗口句柄
  - driver.switch_to.window(handle) # 切换窗口
- 步骤： 
  - 获取当前窗口句柄 
  - 点击链接 启动另一个窗口
  - 获取当前所有窗口句柄
  - 遍历所有窗口句柄
  - 判断当前遍历的窗口句柄不等于当前窗口句柄
  - 切换窗口操作

# 截屏

- 应用场景
  - 失败截图，让错误看的更直观
- 方法
  - driver.get_screenshot_as_file(imgepath)
- 参数
  - imagepath：为图片要保存的目录地址及文件名称
    如： 当前目录 ./test.png
    	    上一级目录 ../test.png

- 扩展
  - 多条用例执行失败，会产生多张图片，可以采用时间戳的形式，进去区分。
  - 操作
      - driver.get_screenshot_as_file("../image/%s.png"%(time.strftime("%Y_%m_%d %H_%M_%S")))
        strftime:将时间转为字符串函数
  - 注意
      - %Y_%m_%d %H_%M_%S：代表，年 月 日 时 分 秒

# 验证码处理

验证码处理方式

- 去掉验证码(项目在测试环境、公司自己的项目)
- 设置万能验证码(测试环境或线上环境，公司自己项目)
- 使用验证码识别技术 (由于现在的验证码千奇百怪，导致识别率太低)
-  使用cookie解决(推荐)

# Cookie处理

- 方法 
  - driver.get_cookies() # 获取所有的cookie
  - driver.add_cookies({字典}) # 设置cookie
- 步骤
  - 打开百度url driver.get("http://www.baidu.com")
  - 设置cookie信息： driver.add_cookie({"name":"BDUSS","value":"根据实际情况编写"})
  - 暂停2秒以上
  - 刷新操作 
- 注意 
  - 以上百度BDUSS所需格式为百度网站特有，别的网站请自行测试
  - 必须进行刷新操作