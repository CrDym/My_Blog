---
title: python学习3-函数
date: 2020-12-18  23:20
tags: Python
categories: Python学习
---

# 函数是什么
函数是组织好的，可以被重复使用，用来实现单一，或者相关联功能的代码，函数的使用可以提高应用的模块性，和代码的复用性
python提供了很多内建函数，比如print(),input()等。我们也可以自己创建函数
<!-- more -->

## 定义一个函数
我们可以定义一个自己想要功能的函数，以下是简单的规则：
- 函数代码块以def关键词开头，后接函数标识符名称和（）
- 任何传入参数和自变量必须放在圆括号中间，圆括号之间可以用于定义参数
- 函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明
- 函数内容以冒号 : 起始，并且缩进
- return [表达式] 结束函数，选择性地返回一个值给调用方，不带表达式的 return 相当于返回 None

## 实例
我们用函数来输出“hello world！”
```python
def hello():
    print("hello world!")
hello()
```

函数中带上参数变量：
- 比较两个数的大小，返回较大的数

```python
def max（a, b）:
    if a > b:
        return a
    else:
        return b
        
a = 1
b = 4

print(max(a,b))
```
输出结果为：
> 4

- 计算面积：

```python
def area(width, height):
    return width*height
w = 4
h = 5
print("width =", w, " height =", h, " area =", area(w, h))
```
输出结果：
> width = 4  height = 5  area = 20

## 函数调用
定义一个函数：给了函数一个名称，指定了函数里包含的参数，和代码块结构。

这个函数的基本结构完成以后，你可以通过另一个函数调用执行，也可以直接执行
## 参数传递
在python中，类型属于对象，变量是没有类型的：

```pytohn
a = [1 ,2 ,3]
a = "Python"
```
以上代码中,[1,2,3]是List类型，“Python”是String类型，而变量a是没有类型的
它只是一个对象的引用（一个指针），以是指向 List 类型对象，也可以是指向 String 类型对象

### 可更改(mutable)与不可更改(immutable)对象
在 python 中，strings, tuples, 和 numbers 是不可更改的对象，而 list,dict 等则是可以修改的对象。
- 不可变类型：变量赋值 a=5 后再赋值 a=10，这里实际是新生成一个 int 值对象 10，再让 a 指向它，而 5 被丢弃，不是改变 a 的值，相当于新生成了 a
- 可变类型：变量赋值 la=[1,2,3,4] 后再赋值 la[2]=5 则是将 list la 的第三个元素值更改，本身la没有动，只是其内部的一部分值被修改了
python 函数的参数传递：
- 不可变类型：类似 C++ 的值传递，如 整数、字符串、元组。如 fun(a)，传递的只是 a 的值，没有影响 a 对象本身。如果在 fun(a)）内部修改 a 的值，则是新生成来一个 a。
- 可变类型：类似 C++ 的引用传递，如 列表，字典。如 fun(la)，则是将 la 真正的传过去，修改后 fun 外部的 la 也会受影响
python中一切皆是对象，严格意义我们不能说值传递还是引用传递，我们应该说传不可变对象和传可变对象。

## 参数
### 必须参数
必需参数须以正确的顺序传入函数。调用时的数量必须和声明时的一样
例如：调用printme()函数时，你必须传入一个参数，不然会出现语法错误

```python
def printme( str ):
   "打印任何传入的字符串"
   print (str)
   return
 
# 调用 printme 函数，不加参数会报错
printme()
```

以上代码的输出结果：
> Traceback (most recent call last):
  File "test.py", line 10, in <module>
    printme()
TypeError: printme() missing 1 required positional argument: 'str'
### 关键字参数
关键字参数和函数调用关系紧密，函数调用使用关键字参数来确定传入的参数值。

使用关键字参数允许函数调用时参数的顺序与声明时不一致，因为 Python 解释器能够用参数名匹配参数值。
以下实例在函数 printme() 调用时使用参数名：

```python
def printme( str ):
   "打印任何传入的字符串"
   print (str)
   return
 
#调用printme函数
printme( str = "python")
```
以下实例中演示了函数参数的使用不需要使用指定顺序：

```python
def printinfo( name, age ):
   "打印任何传入的字符串"
   print ("名字: ", name)
   print ("年龄: ", age)
   return
 
#调用printinfo函数
printinfo( age=28, name="python" )

```

以上代码输出结果：

> 名字： python
> 年龄： 25
### 默认参数
调用函数时，如果没有传递参数，则会使用默认参数。以下实例中如果没有传入 age 参数，则使用默认值：


```python
def printinfo( name, age = 35 ):
   "打印任何传入的字符串"
   print ("名字: ", name)
   print ("年龄: ", age)
   return
 
#调用printinfo函数
printinfo( age=28, name="python" )
print ("------------------------")
printinfo( name="python" )
```
### 不定长参数
你可能需要一个函数能处理比当初声明时更多的参数。这些参数叫做不定长参数，和上述 2 种参数不同，声明时不会命名。
加了星号 * 的参数会以元组(tuple)的形式导入，存放所有未命名的变量参数。

```python
def printinfo( arg1, *vartuple ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   print (vartuple)
 
# 调用printinfo 函数
printinfo( 70, 60, 50 )
```

> 输出：
> 70
> （60，50）

如果在函数调用时没有指定参数，它就是一个空元组。我们也可以不向函数传递未命名的变量。如下实例：

```python
def printinfo( arg1, *vartuple ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   for var in vartuple:
      print (var)
   return
 
# 调用printinfo 函数
printinfo( 10 )
printinfo( 70, 60, 50 )
```

> 输出:
> 10
> 输出:
> 70
> 60
> 50

还有一种就是参数带两个星号
加了两个星号**的参数会以字典的形式导入


```python
def printinfo( arg1, **vardict ):
   "打印任何传入的参数"
   print ("输出: ")
   print (arg1)
   print (vardict)
 
# 调用printinfo 函数
printinfo(1, a=2,b=3)
```

输出结果：
> 1
> {'a': 2, 'b': 3}