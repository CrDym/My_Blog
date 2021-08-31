---
title: Homebrew相关
date: 2021-08-19  22:28
tags: Mac使用
categories: 杂七杂八

---

# 概念

- Mac的软件包管理工具，类似于linux的`apt-get`，能在mac中方便地安装软件或者卸载软件。

# 安装Homebrew

## 安装

- Homebrew依赖xcode和其Command Line Tools。

  1. 在App Store中安装最新版本的xcode；
  2. 执行`xcode-select --install`安装Command Line Tools。

- 把Homebrew安装到`/usr/local`。

  

  ```linux
  /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
  ```

## 卸载



```linux
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```

## 重装

1. 备份`/usr/local/Cellar`。

2. 删除Homebrew相关文件。

   

   ```linux
   cd /usr/local
   sudo rm -rf Library .git .gitignore bin/brew README.md share/man/man1/brew
   sudo rm -rf Homebrew
   sudo rm -rf ~/Library/Caches/Homebrew
   ```

3. 卸载Homebrew。

   

   ```linux
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
   ```

4. 安装Homebrew。

   

   ```linux
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   ```

5. 将第1步中的备份拷贝回`/usr/local/Cellar`。

6. 更新Homebrew及其管理的各软件。

   

   ```linux
   brew update
   brew upgrade
   brew cleanup
   ```

7. `brew doctor`检测Homebrew潜在问题，并自行排错。如使用`brew link 软件名`将备份的软件重新symlink到Homebrew上。

# 使用Homebrew

## 安装软件

`brew install 软件名`，如`brew install git`。

## 卸载软件

`brew uninstall 软件名`，如`brew uninstall git`。

## 查找软件

```
brew search 查询内容
```

1. 普通查询，`brew search git`
2. 正则查询，`brew search /gi*/`

## 查看哪些安装包需要更新

```
brew outdated
```

## 升级软件

- `brew upgrade 软件名`：更新指定软件，如`brew update git`。
- `brew upgrade`：更新所有软件。

## 清理软件

- `brew cleanup -n`：查看哪些软件包要被清除。
- `brew cleanup 软件名`：清除指定软件包的所有老版本。
- `brew cleanup`：清除所有软件包的所有老版本。

## 关联软件

- `brew prune`：清理无用的symlink，且清理与之相关的位于`/Applications`和`~/Applications`中的无用App链接。

- `brew link 软件名`：将指定软件的安装文件symlink到Homebrew上。

  > `brew install`安装的软件会自动执行link操作；
  >  DIY安装的需要手动执行link操作；
  >  加上`--overwrite`选项，会先删除旧的symlink，再进行新的link操作。

## 信息查询

- `brew -v`：查看Homebrew版本号。
- `brew list`：列出已安装的软件。
- `brew home`：用浏览器打开homebrew官网。
- `brew info`：显示软件信息。

## 其他操作

- `brew update`：升级Homebrew自身。
- `brew doctor`：检测系统中与Homebrew有关的潜在问题。



