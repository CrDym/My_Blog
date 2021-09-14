---
title: Selenium系列2-搭建测试环境
date: 2021-09-09  22:28
tags: UI自动化
categories: 自动化测试
---

## 浏览器驱动下载

### ChromeDriver for Chrome

首先查看Chrome版本号

![image-20210911160205395](https://img.rockche.cn//image-20210911160205395.png)

下载对应版本的驱动

- 官方下载地址：http://chromedriver.storage.googleapis.com/index.html
- 国内镜像地址：https://npm.taobao.org/mirrors/chromedriver
- 版本对应查看地址：http://chromedriver.storage.googleapis.com/2.43/notes.txt

选择指定的ChromeDriver版本，可根据不同的平台（Win、Mac、Linux）下载指定的ChromeDriver

![image-20210911160338218](https://img.rockche.cn//image-20210911160338218.png)

### Geckodriver for Firefox

使用Firefox进行自动化测试时，Selenium1.0或Selenium2.0是可以直接驱动Firefox的，如果使用Selenium3.0，则需要下载Geckodriver驱动

- 驱动下载地址：https://github.com/mozilla/geckodriver/releases
- 国内镜像地址：https://npm.taobao.org/mirrors/geckodriver/

根据不同的平台（Win、Mac、Linux等）下载指定的geckodriver驱动
<!-- more -->

![image-20210911161011883](https://img.rockche.cn//image-20210911161011883.png)

> - `Firefox 47` 及以前版本，不需要`geckodriver`驱动。
> - `geckodriver v0.19.0`：`Firefox 55`（及更高版本），`Selenium3.5`（及更高）
> - `geckodriver v0.21.0`：`Firefox 57`（及更高版本），`Selenium3.11`（及更高）

### IEDriverServer for IE

IEDriverServer下载地址：http://selenium-release.storage.googleapis.com/index.html

根据Win平台是32位还是64位，下载指定的IEDriverServer驱动

![image-20210911161338542](https://img.rockche.cn//image-20210911161338542.png)

### Webdriver for Edge

- 驱动下载地址：https://developer.microsoft.com/zh-cn/microsoft-edge/tools/webdriver/

选择指定的WebDriver版本，可根据不同的平台（Win、Mac、Linux）下载指定的WebDriver

![image-20210911162124182](https://img.rockche.cn//image-20210911162124182.png)

### OperaDriver for Opera

- 驱动下载地址：https://github.com/operasoftware/operachromiumdriver/releases
- 国内镜像地址：https://npm.taobao.org/mirrors/operadriver/

![image-20210911162340870](https://img.rockche.cn//image-20210911162340870.png)

## 浏览器驱动与Python整合

- Windows环境

  将下载好的浏览器驱动解压后，如：`chromedriver.exe`放置在Python安装路径的根目录下即可

- Mac环境

  mac下需要将驱动解压后放在`/usr/local/bin/`目录中

## 安装Selenium

通过pip命令可以直接安装

```shell
pip install selenium
```

## 第一个测试脚本

完成了以上的准备工作，我们的Selenium+Python自动化测试环境就搭建好了，下面就可以编写自动化脚本了

```python
# 1.导入selenium包
from selenium import webdriver
from time import sleep

# 2.选择并打开浏览器（谷歌）
driver = webdriver.Chrome()

# 3. 输入百度网址
driver.get("http://www.baidu.com")
sleep(3)

# 4.对网址的操作
# 5.关闭浏览器
driver.quit()
```

## 拓展

### Chrome模拟移动端

```python
# 1.导入selenium包
from selenium import webdriver
from time import sleep

# 2.添加谷歌浏览器加载项
mobileEmulation = {"deviceName": "iPhone X"}
options = webdriver.ChromeOptions()
# 因为传入的是字典类型的数据，所以使用的add方法也不一样
options.add_experimental_option("mobileEmulation", mobileEmulation)

# 3.打开谷歌浏览器——将模拟移动端的参数，传入打开的浏览器中
# options和chrome_options一样，chrome_options将弃用。
driver = webdriver.Chrome(options=options)

# 4.打开地址
url = "http://www.baidu.com"
driver.get(url)
sleep(3)

# 5.关闭浏览器
driver.quit()
```

## 参考

https://www.cnblogs.com/liuyuelinfighting/p/14921476.html

