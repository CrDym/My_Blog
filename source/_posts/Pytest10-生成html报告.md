---
title: Pytest10-生成html报告
date: 2021-07-03  23:45
tags: Pytest
categories: Python学习
---

## 前言

在pytest中，如何生成html测试报告呢，pytest提供了pytest-html插件，可以帮助我们生成测试报告，当然，如果希望生成更加精美的测试报告，我们还可以使用allure生成报告，下面我们就来详细看看如何实现吧

## pytest-html插件
### 插件安装

pip命令安装

```shell
pip install pytest-html
```

### 使用实例

使用方法很简单，在测试用例的目录下执行命令 `pytest --html=reportname.html` 即可

生成的报告效果如下：

![image-20210705141619343](https://img.rockche.cn//image-20210705141619343.png)

#### 合并css

使用上面的命令生成报告后，css是独立的，分享报告出去的时候样式会丢失，我们可以使用如下命令把css样式合并到html里

```shell
pytest --html=report.html --self-contained-html
```

## allure生成报告

### allure介绍

Allure 是一款轻量级的开源自动化测试报告生成框架。它支持绝大部分测试框架，比如 TestNG、Junit 、Pytest、unittest 等

### 安装allure

#### Windows下安装

1.因为allure依赖于java环境，所以必须先安装java环境并设置环境变量，此处略过

2.在github上下载最新版本：https://github.com/allure-framework/allure2/releases

![image-20210705150235745](https://img.rockche.cn//image-20210705150235745.png)

3.解压后，打开`\bin`文件夹，会看到`allure.bat`文件，将此路径添加到环境变量
4.cmd输入`allure`出现帮助信息，表示安装成功

#### Mac OS X下安装

使用命令`brew install allure`安装

### 安装pytest-allure-adaptor插件

使用命令`pip install allure-pytest`

### 生成xml格式报告

在运行用例的目录下执行 `pytest -s -q --alluredir ./report/xml`

> 1. '-s':指的是快速执行
> 2. '-q':静默执行，删除多余的执行内容信息
> 3. '--alluredir':用例执行的目录
> 4. './report/xml':报告xml的存放地址，不指定默认在当前目录自动生成

### 生成html格式报告

 使用命令`allure generate report/xml -o report/html`

> 1. `report/xml` 指的是xml文件的目录
> 2. `report/html` 指的是html文件的目录

注意：xml文件目录与html文件目录不能相同，必须指定一个空的目录生成最后的html报告

### 效果展示

![image-20210705153835953](https://img.rockche.cn//image-20210705153835953.png)

## 总结

以上便是pytest生成测试报告的两种方法了，关于allure的详细内容，将在后续的文章中介绍

