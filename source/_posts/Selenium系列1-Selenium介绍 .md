---
title: Selenium系列1-Selenium介绍 
date: 2021-03-31  22:28
tags: UI自动化
categories: 自动化测试
---

## 主流的自动化测试工具

- QTP
  - QTP是一个商业的自动化测试工具，收费，支持web、桌面自动化测试
- Selenium
  - Selenium是一个开源的web自动化测试工具，免费
- Robot framework
  - Robot framework是一个基于Python可扩展的关键字驱动的测试自动化框架（基本上已被淘汰）
- Airtest
  - Airtest是网易公司的基于图像识别的UI自动化测试工具，多用于测试手机游戏，也可以用于APP、小程序测试

## Selenium介绍

Selenium是最广泛使用的开源Web UI（用户界面）自动化测试套件之一。它最初由杰森·哈金斯（Jason Huggins）于2004年开发，作为Thought Works的内部工具。Selenium支持跨不同浏览器，平台和编程语言的自动化。

Selenium可以轻松部署在Windows，Linux，Solaris和Macintosh等平台上。此外，它支持IOS（IOS，Windows Mobile和Android）等移动应用程序的OS（操作系统）。

Selenium通过使用特定于每种语言的驱动程序支持各种编程语言。Selenium支持的语言包括C#，Java，Perl，PHP，Python和Ruby。目前，Selenium Web驱动程序最受Python和C＃欢迎。 Selenium测试脚本可以使用任何支持的编程语言进行编码，并且可以直接在大多数现代Web浏览器中运行。 Selenium支持的浏览器包括Internet Explorer，Mozilla Firefox，Google Chrome和Safari。

## Selenium特点

- 开源
- 跨平台：Linux、windows、Mac
- 支持多种浏览器：Firefox、Chrome、IE、Edge、Opera、Safari等
- 支持多种语言：java、python、C#、Javascript、Ruby、Php等
- 成熟稳定
- 功能强大
<!-- more -->

## Selenium的发展

从2004年至今，Selenium经历了3个版本，即`Selenium1.0`，`Selenium2.0`，`Selenium3.0`。

- Selenium1.0
  - seleniumIDE      
    - 是Firefox浏览器中的一个插件，实现简单的浏览器操作的录制与回放功能。生成测试用例，可将测试用例转换为其他语言的自动化脚本。如果没有编程经验，可以通过Selenium IDE来快速熟悉Selenium的命令。（只适用于火狐浏览器）
  - seleniumGrid 
    - 分布式测试。用于运行在不同的机器，不同的浏览器并行测试的工具，目的在于加快测试用例运行的速度，从而减少测试运行的总时间。利用Grid可以很方便地实现在多台机器上和异构环境中运行测试用例。
  - seleniumRC
    - Selenium RC是Selenium1.0核心部分，Selenium RC的功能就是通过代码操作浏览器。
- Selenium2.0    
  - Selenium1.0+WebDriver
    - WebDriver比Selenium RC功能强大且简单。WebDriver是通过原生浏览器支持或者浏览器扩展来直接控制浏览器。WebDriver针对各个浏览器而开发，使用不同浏览器都需要对应浏览器驱动，与浏览器紧密集成，因此支持创建更高级的测试，避免了JavaScript安全模型导致的限制。除了来自浏览器厂商的支持之外，WebDriver还利用操作系统级的调用，模拟用户输入。我们在使用WebDriver时，可以看到，是先启动了浏览器对应driver，通过浏览器driver启动浏览器。
- Selenium3.0
  - 弃掉了seleniumRC
  - 全面拥抱java8
  - 支持MacOS
  - 通过Mozilla官方的geckodriver来支持firefox
  - Selenium 3.0在Selenium 2.0的基础上增加了对Win10系统的Edge浏览器和Mac系统Safari浏览器的支持，并且在启动Firefox浏览器时也必须使用浏览器驱动geckodriver。去掉了Selenium RC，因此Selenium 3.0的学习核心也是WebDriver。

## WebDriver与Selenium RC的区别

**Selenium RC**

Selenium RC使用的是javascript注入的方式跟浏览器打交道。这样Selenium RC需要启动一个Server，然后将操作页面元素的API 转成javascript脚本，再把这段脚本注入到浏览器中去执行。而通过这种javascript注入的方式一来太依赖翻译成javascript质量的好坏，二来javascript存在同源问题。这使测试变得不那么容易。

总结：

- Selenium RC需要Selenium Server才能运行测试用例。
- Selenium RC使用JavaScript来驱动浏览器运行测试用例。
- Selenium RC只能支持Web应用的测试。
- Selenium RC能支持所有浏览器但并不能及时支持最新版本。

**WebDriver**

与Selenium RC不同的是Selenium WebDriver针对不同的浏览器进行独立开发Driver，利用浏览器的原生API去直接操作浏览器和页面元素，这样大大提高了测试的稳定性和速度。当然因为不同的浏览器对Web元素操作和呈现多多少少会存在一些差异，这也就造成现在不同的浏览器需要有对应不同的Driver。

总结：

- WebDriver不需要Selenium Server就可以运行测试用例。
- WebDriver独立使用原生浏览器来运行测试用例。
- WebDriver既可以测试传统桌面Web应用，也可以测试手机上的应用程序，如iPhone或Android上的app程序。
- WebDriver能支持大多数浏览器的最新版本。

**总结**

| Selenium RC                                                  | Selenium WebDirver                                           |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| Selenium RC的架构复杂，运行测试脚本前必须先启动Selenium RC Server | WebDirver架构简单，通过OS层级来控制浏览器                    |
| Selenium RC通过Selenium RC Server中转才能与浏览器进行交互    | WebDriver直接与浏览器进行交互                                |
| Selenium RC则通过Selenium Core（javascript实现）来间接驱动浏览器 | Webdriver直接调用浏览器原生API进行驱动，速度较快             |
| Selenium RC的API复杂冗余，不利于学习掌握                     | Webdriver的API简洁，只要掌握几个常用的即可进行测试           |
| Selenium RC只能驱动可视化的浏览器                            | Webdriver除了驱动可视化的浏览器，还可以驱动内存模式的浏览器，比如HtmlUnit browser，phantomjs |
| 不能测试移动应用程序                                         | 可以测试iPhone/Android应用程序                               |

## WebDirver工作原理

![image-20210911153057800](https://img.rockche.cn//image-20210911153057800.png)

## 参考

https://www.cnblogs.com/liuyuelinfighting/p/14901498.html
