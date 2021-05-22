---
title: Pipenv虚拟环境
date: 2021-05-12  16:12
tags: Python
categories: Python学习
---
# 什么是Pipenv
Pipenv是python官方推荐的虚拟环境管理工具，可以把它当作是virtualenv，pip，pyenv三者的集合工具，类似于npm和composer。
它能够自动为项目创建和管理虚拟环境，从 Pipfile 文件中添加或者删除包，同时生成 Pipfile.lock 文件来锁定安装包的版本和依赖信息，避免构建错误。

pipenv 主要解决了以下问题：
- 不用再单独使用 virtualenv、pyenv 和 pip 了，现在它们结合到了一起。
- 不用再维护 requirement.txt 了，使用 Pipfile 和 Pipfile.lock 来代替。
- 可以在开发环境使用多个 python 版本。
- 在安装的 pyenv 条件下，可以自动安装需要的 python 版本。
- 安全，广泛地使用 Hash 校验，能够自动曝露安全漏洞。
- 随时查看图形化的依赖关系。
- 可通过自动加载 .env 读取环境变量，简化开发流程。
<!-- more -->

# 安装 pipenv
## MacOS下使用pip安装

```shell
$ pip install --user pipenv
```
这个命令在用户级别（非系统全局）下安装 pipenv。如果安装后 shell 提示找不到 pipenv 命令，你需要添加当前 Python 用户主目录的 bin 目录到 PATH 环境变量。如果你不知道 Python 用户主目录在哪里，用下面的命令来查看：
```shell
$ python -m site --user-base
```
输出如下
![](https://img.rockche.cn/mweb/16216670258674.jpg)

## MacOS下使用brew安装
```shell
$ brew install pipenv
```
# Pipenv常用命令
## 命令

```
$ pipenv
Usage: pipenv [OPTIONS] COMMAND [ARGS]...

Options:
  --where          显示项目文件所在路径
  --venv           显示虚拟环境实际文件所在路径
  --py             显示虚拟环境Python解释器所在路径
  --envs           显示虚拟环境的选项变量
  --rm             删除虚拟环境
  --bare           最小化输出
  --completion     完整输出
  --man            显示帮助页面
  --three / --two  使用Python 3/2创建虚拟环境（注意本机已安装的Python版本）
  --python TEXT    指定某个Python版本作为虚拟环境的安装源
  --site-packages  附带安装原Python解释器中的第三方库
  --jumbotron      An easter egg, effectively.
  --version        版本信息
  -h, --help       帮助信息

```
## 命令参数

```
Commands:
  check      检查安全漏洞
  graph      显示当前依赖关系图信息
  install    安装虚拟环境或者第三方库
  lock       锁定并生成Pipfile.lock文件
  open       在编辑器中查看一个库
  run        在虚拟环境中运行命令
  shell      进入虚拟环境
  uninstall  卸载一个库
  update     卸载当前所有的包，并安装它们的最新版本
```

# pipenv 使用
## 创建环境，安装指定版本的python
首先需要看一下当前目录是否已经创建了虚拟环境，使用如下命令
```
$ pipenv -venv
```
结果如下
![](https://img.rockche.cn/mweb/16216685349589.jpg)
这说明当前的项目没有创建虚拟环境，可以使用Pipenv 来创建一个虚拟环境：

```
$ pipenv –three
```
如果指定了 --two 或者 --three 选项参数，则会使用 python2 或者 python3 的版本安装，否则将使用默认的 python 版本来安装。当然也可以指定准确的版本信息：

```
$ pipenv install --python 3
$ pipenv install --python 3.8
$ pipenv install --python 2.7.14
```
pipenv 会自动扫描系统寻找合适的版本信息，如果找不到的话，同时又安装了 pyenv 的话，则会自动调用 pyenv 下载对应版本的 python， 否则会报错。

这时候在当前项目根目录下会生成 Pipfile 和 Pipfile.lock 两个环境初始化文件
## 进入|退出环境
 进入环境
`pipenv shell`
 退出环境
`exit`
## 安装第三方包
测试安装 selenium 包

`pipenv install selenium`

此时，Pipfile 里有最新安装的包文件的信息，如名称、版本等。用来在重新安装项目依赖或与他人共享项目时，你可以用 Pipfile 来跟踪项目依赖。

Pipfile 是用来替代原来的 requirements.txt 的，内容类似下面这样。source 部分用来设置仓库地址，packages 部分用来指定项目依赖的包，dev-packages 部分用来指定开发环境需要的包，这样分开便于管理。

```shell
$ cat Pipfile

[[source]]
name = "pypi"
url = "https://pypi.tuna.tsinghua.edu.cn/simple/"
verify_ssl = true


[packages]
urllib3 = "*"
selenium = "*"

[dev-packages]

[requires]
python_version = "3.8"
```

Pipfile.lock 则包含你的系统信息，所有已安装包的依赖包及其版本信息，以及所有安装包及其依赖包的 Hash 校验信息。

```shell
$ Pipfile.lock
{
    "_meta": {
        "hash": {
            "sha256": "b02856f549692af24a14f79bfe022df973a9ea09ddfb4cbf7c7ecab5248ac322"
        },
        "pipfile-spec": 6,
        "requires": {
            "python_version": "3.8"
        },
        "sources": [
            {
                "name": "pypi",
                "url": "https://pypi.tuna.tsinghua.edu.cn/simple/",
                "verify_ssl": true
            }
        ]
    },
    "default": {
        "selenium": {
            "hashes": [
                "sha256:2d7131d7bc5a5b99a2d9b04aaf2612c411b03b8ca1b1ee8d3de5845a9be2cb3c",
                "sha256:deaf32b60ad91a4611b98d8002757f29e6f2c2d5fcaf202e1c9ad06d6772300d"
            ],
            "index": "pypi",
            "version": "==3.141.0"
        },
        "urllib3": {
            "hashes": [
                "sha256:2f4da4594db7e1e110a944bb1b551fdf4e6c136ad42e4234131391e21eb5b0df",
                "sha256:e7b021f7241115872f92f43c6508082facffbd1c048e3c6e2bb9c2a157e28937"
            ],
            "index": "pypi",
            "version": "==1.26.4"
        }
    },
    "develop": {}
}

```
那么Pipfile 和 Pipfile.lock 有什么用呢？
Pipfile 其实一个 TOML 格式的文件，标识了该项目依赖包的基本信息，还区分了生产环境和开发环境的包标识，作用上类似 requirements.txt 文件，但是功能更为强大。
Pipfile.lock 详细标识了该项目的安装的包的精确版本信息、最新可用版本信息和当前库文件的 hash 值，顾明思义，它起了版本锁的作用。
可以注意到当前 Pipfile.lock 文件中的 Selenium 版本标识为 ==3.141.0，意思是当前我们开发时使用的就是 3.141.0版本，它可以起到版本锁定的功能。

举个例子，刚才我们安装了 Selenium 3.141.0 的版本，即目前（2021.5.22）的最新版本。但可能 Selenium 以后还会有更新，比如某一天 Selenium 更新到了 3.2 版本，这时如果我们想要重新部署本项目到另一台机器上，假如此时不存在 Pipfile.lock 文件，只存在 Pipfile文件，由于 Pipfile 文件中标识的 Selenium 依赖为 selenium= “*”，即没有版本限制，它会默认安装最新版本的 Selenium，即 3.2，但由于 Pipfile.lock 文件的存在，它会根据 Pipfile.lock 来安装，还是会安装 Selenium 3.141.0，这样就会避免一些库版本更新导致不兼容的问题。
## 安装指定版本包
`pipenv install selenium==3.141.0
`
## 安装开发环境下的包
通常有一些Python包只在你的开发环境中需要，而不是在生产环境中，例如单元测试包。 加 --dev 表示包括 Pipfile 的 dev-packages 中的依赖。

`pipenv install unittest --dev`

django库现在将只在开发虚拟环境中使用。如果你要在你的生产环境中安装你的项目：

`pipenv install`

这不会安装unittest包。
但是，如果有一个开发人员将你的项目克隆到自己的开发环境中，他们可以使用–dev标志，将django也安装：

`pipenv install –dev`

也就是说一个–dev参数，帮你在同一个虚拟环境中又区分出了开发和非开发环境

## 卸载第三方包

```shell
pipenv uninstall selenium //或者 pipenv uninstall --all
```
## 更新安装包

```
pipenv update selenium 
pipenv update # 更新所有安装包
```

## 查看虚拟环境目录
`$ pipenv --venv`

## 查看项目根目录
`$ pipenv --where`
## 检查软件包的完整性
`$  pipenv check`
## 生成Pipfile.lock
有时候可能 Pipfile.lock 文件不存在或被删除了，可以使用如下命令生成：
`$ pipenv lock`
## 修改Pipenv下载源
在使用pipenv install安装的过程中如果下载比较慢可以在pipfile文件中指定下载源：

```shell
[[source]]
name = "pypi"
url = "https://pypi.tuna.tsinghua.edu.cn/simple/"
verify_ssl = true


[packages]
urllib3 = "*"
selenium = "*"

[dev-packages]

[requires]
python_version = "3.8"
```
Pip下载源
- 阿里: http://mirrors.aliyun.com/pypi/simple/
- 豆瓣: http://pypi.douban.com/simple/
- 清华: https://pypi.tuna.tsinghua.edu.cn/simple

## 查看依赖树
`$ pipenv graph`
![](https://img.rockche.cn/mweb/16216706361891.jpg)
## 锁定版本
更新 lock 文件锁定当前环境的依赖版本

`$ pipenv lock`

# 参考链接
[Pipenv – 超好用的 Python 包管理工具](https://segmentfault.com/a/1190000015389565)
[PyCharm+Pipenv虚拟环境开发和依赖管理的教程详解](https://cloud.tencent.com/developer/article/1739844)


