---
title: Linux2-测试常用命令
date: 2021-07-19  22:12
tags: Linux
categories: Linux学习

---

## 前言

我们知道 Linux 下有非常多的命令，我们可以通过这些命令在 Shell 环境下与机器进行交互，那么 Linux 下有多少资源可以供我们调度呢？其实是非常多的，但所有资源都可以分为三大类型。

> 文件：Everything is file；
>
> 进程：文件的运行形态
>
> 网络：特殊的文件

下面我们就来看看有哪些测试常用的命令吧

## cd命令

### 进入上级目录

```bash
cd ..
```

### 进入当前用户主目录

```bash
cd ～
```

### 进入根目录

```bash
cd /
```

### 进入上两级目录

```bash
cd ../..
```

### 进入当前目录

```bash
cd .
```

### 进入指定目录，如/usr/local

```bash
cd /usr/local
```



## mkdir命令

### 新建一个文件夹

```bash
mkdir test
```

### 新建多个文件夹

```bash
mkdir test1 test2 test3
```

### 新建一个多层级文件夹

```bash
mkdir -p test/rock/good
```

### 新建一个文件夹并设置权限

```bash
mkdir -m 777 test
```



## mv命令

### 移动文件test,py到/usr/local

```bash
mv test.py  /usr/local
```

### 移动文件test,py到/usr/local并重命名为test1.py

```bash
mv test.py  /usr/local/test1.py
```

### 移动文件到上级目录

```bash
mv test.py ../
```

### 移动文件test.py和test2.py到/usr/local

```bash
mv test.py test2.py -t  /usr/localbash
```

### 移动文件test.py到/usr/local下，但/usr/local已存在test.py，强制覆盖

```bash
mv test.py  -f  /usr/local
```

### 移动文件test.py到/usr/local下，但/usr/local已存在test.py，询问是否覆盖

```bash
mv test.py  -i  /usr/local
```



## cp命令

### 复制文件test.txt到/usr/local目录

```bash
cp test.txt /usr/local
```

### 复制文件夹 Test到/usr/local目录

```bash
cp -r Test/ /usr/local
```

### 再次复制文件test.txt到/usr/local目录，强制覆盖

```bash
cp -f test.txt /usr/local
```

### 再次复制文件test.txt到/usr/local目录，询问是否强制覆盖

```bash
cp -i test.txt /usr/local
```

### 复制文件tests.txt到/usr/local目录，并把修改时间和访问权限也复制

```bash
cp -p test.txt /usr/local
```



## history命令

### 查看历史命令执行记录 

```bash
history 
```

### 查看命令mkdir 的历史执行记录 

```bash
history | grep mkdir 
```

![image-20210720163725599](https://img.rockche.cn//image-20210720163725599.png)

### 执行历史记录中，序号为1280的命令 

```bash
!1280
```

### 执行上一条命令

```bash
！！
```

### 查找最后10条历史记录

```bash
history 10
```

或

```bash
history | tail -10
```

![image-20210720164100633](https://img.rockche.cn//image-20210720164100633.png)

### 清除历史记录

```bash
history -c
```

### 把历史记录写入文件中

```bash
history -w
cat ~/.bash_history
```



## tar命令

### 压缩一个文件 Test.ini 

```bash
tar -zcvf Test.tar.gz Test.ini 
```

### 压缩多个文件 Test.ini、 readme.ini

```bash
tar -zcvf all.tar.gz Test.ini readme.ini
```

### 压缩文件夹 Test/

```bash
tar -zcvf Test.tar.gz Test/
```

### 将当前目录，所有jpg文件打包成Testjpg.tar

```bash
tar -cvf Testjpg.tar.gz *.jpg
```

### 将当前目录，所有jpg文件打包成Testjpg.tar.gz

```bash
tar -zcvf Testjpg.tar.gz *.jpg
```

### 解压 Testjpg.tar

```bash
tar -xvf Testjpg.tar
```

### 解压 Testjpg.tar.gz

```bash
tar -zxvf Testjpg.tar.gz
```



## tail命令

#### 实时刷新log

```shell
tail -f test.log
```

#### 实时刷新最新50条log

```bash
tail -50f test.log
```

#### 显示最后5条log

```bash
tail -n 5 test.log
tail -5 test.log
```

#### 显示第5条后面的所有log

```bash
tail -n +5 test.log
```



## ls命令

###  列出当前目录中所有的子目录和文件(不包含隐藏文件 .开头的)

```bash
ls
```

### 列出目录下的所有的子目录和文件（包含隐藏文件 .开头的）

```bash
ls -a
```

### 列出文件的详细信息（包括权限、所有者、文件大小等） 两种方式

```bash
ls -l
ll
```

### 列出当前目录中所有以“test”开头的详细内容

```bash
ls -l test*
```

### 按文件最后修改时间降序排列显示

```bash
ls -t
```

###  按文件大小从大到小排序显示

```bash
ls -S
```

###  查看文件时显示文件大小

```bash
ls -l -h
ll -h
```

## ps命令

### 查看所有进程

```bash
ps -A
```

### 显示所有进程的详细信息

```bash
ps -ef
```

![image-20210721171505481](https://img.rockche.cn//image-20210721171505481.png)

**ps -ef 各个字段含义**

> UID：表示用户ID
>
> PID：表示进程ID
>
> PPID：表示父进程号
>
> C：表示CPU的占用率
>
> STIME：进程的启动时间
>
> TTY：登入者的终端机位置
>
> TIME：表示进程执行起到现在总的CPU占用时间
>
> CMD：表示启动这个进程的命令

### 查找特定进程

```bash
ps -ef | grep python
```

### 显示所有进程更详细的信息，包括进程占用CPU、内存使用率

```bash
ps -aux
```

![image-20210721172209531](https://img.rockche.cn//image-20210721172209531.png)

**ps -aux 各个字段含义**

> USER：表示哪个用户启动了这个进程
>
> PID ：进程ID
>
> %CPU：进程CPU的占用率
>
> %MEM：进程物理内存的占用率
>
> VSZ ：进程占用的虚拟内存量 (Kbytes)
>
> RSS ：进程当前实际上占用了多少内存
>
> TTY ：进程是在哪个终端机上面运作，若与终端机无关，则显示 ?
>
> STAT：该程序目前的状态，主要的状态有
>
> - 　　R ：运行；该程序目前正在运作，或者是可被运作
> - 　　D：不可中断：一般是IO进程
> - 　　S ：中断；该程序目前正在睡眠当中 (可说是 idle 状态)，但可被某些讯号 (signal) 唤醒。
> - 　　T ：停止：该程序目前正在侦测或者是停止了
> - 　　Z ：僵尸：该程序应该已经终止，但是其父程序却无法正常的终止他，造成 zombie (僵尸) 程序的状态
>
> START：该进程启动的时间点
>
> TIME ：进程从启动后到现在，实际占用CPU的总时间
>
> COMMAND：启动该进程的命令

### 根据CPU、内存使用率降序排列

```bash
ps -aux --sort -pcpu
ps -aux --sort -pmem
```

## top命令

### 查看所有进程的资源占用情况

top命令是Linux下常用的性能分析工具，能够实时显示系统中各个进程的资源占用状况，类似于Windows的任务管理器

```bash
top
```

### 监控每个逻辑CPU的状况

```bash
top  ，按 1
```

### 高亮显示当前运行进程

```bash
top ，按 b 
```

### 显示 完整命令

```bash
top ，按 c
```

### 切换显示CPU

```bash
top，按t
```

### 按CPU使用率从大到小排序

```bash
top，按P
```

### 切换显示Memory

```bash
top，按m
```

### 按Memory占用率从大到小排序

```bash
top，按M
```

### 按累计运行时间Time从大到小排序

```bash
top，按T
```

### 高亮CPU列

```bash
top，按x
```

### 彩色高亮显示

```bash
top，按z
top，按shift+z 可以调配色方案
```

### **通过”shift + >”或”shift + <”可以向右或左改变排序列**

```bash
top shift + >或shift + <
```

### 忽略闲置和僵死进程

```bash
top，按i
```

### 杀掉进程

```bash
top，按k，输入PID
```

### 改变内存的显示单位，默认为KB

```bash
top，按e （针对列表）
top，按E （针对头部统计信息）
```

### 退出top程序

```bash
按 q
```



## wget命令

### 下载test.jpg文件

```bash
wget http://xxx（文件地址）
```

### 下载test.jpg文件，并存储名为test_demo.jpg

```bash
wget -o test_demo.jpg http://xxx（文件地址）
```

### 下载test.jpg文件，后台形式下载

```bash
wget -b http://xxx（文件地址）
```



## rm命令

### 删除/root/Test/目录下的文件Test.ini （系统会询问是否删除）

```bash
rm /root/Test/Test.ini
```

### 强制删除/root/Test/目录下的文件Test.ini（直接删除，系统不会提示）

```bash
rm -f /root/Test/Test.ini
```

### 删除/root/Test/目录下的所有.log文件

```bash
rm -f /root/Test/*.log
```

### 删除/root/Test/目录下的 demo/文件夹

```bash
rm -r /root/Test/demo/
```

### 强制删除/root/Test/目录下的 demo/文件夹

```bash
rm -rf /root/Test/demo/
```

### 删除/root/Test/目录下的所有内容

```bash
rm -rf /root/Test/*
```



## 创建文件命令

### touch

#### 创建一个文件
```bash
touch Test.ini  
```
####  同时创建两个文件
```bash
touch test1.txt test2.txt
```
#### 批量创建文件（如创建2000个文件）
```bash
touch test{0001..2000}.txt
```
### vi和vim

```bash
vim test.txt
```

#### 使用方式

> 进入文档后，点击 i 进入insert模式，在文档中输入文字，在当前光标处编辑，文档下面会有insert的标识
> 进入文档后，点击 a 可以编辑光标下一位
> 退出编辑状态后，输入 Shift + g 即可立刻跳转到本文档最后
> 点击 esc 按钮可以退出编辑状态
> : 输入冒号可以输入文档相关的指令
> wq 表示保存并退出
> q 表示退出
> q! 强制退出，不保存修改的内容
> 退出编辑状态，点击 x 键可以删除1个字符，一次有效
> 退出编辑状态，点击 dd 可以删除一行字符
> 退出编辑状态，点击 r + 要替换的内容，即可将当前内容替换

### 使用>、>>

#### >

直接覆盖原文件，不会有任何提示

#### >>

追加在原文件末尾，不会覆盖原文件的内容

#### 直接用>创建空文件

```bash
> test.ini
```

#### ls 创建文件（将结果写入文件）

```bash
ls > test.ini
ls >> test.ini
```

#### grep 创建文件（将结果写入文件）

```bash
ps -ef | grep java >test.ini
ps -ef | grep java >>test.ini
```

#### echo 创建文件（将结果写入文件）

```bash
echo $PATH > test.ini
echo $PATH >> test.ini
```

#### cat 创建文件

```bash
cat 1.txt > 2.txt	
```

cat命令可以一次显示整个文件，如果文件比较大，使用不是很方便；适用于文件内容少的情况。

#### cd 创建文件

```bash
cd > test.txt
```

cd最主要的作用是切换目录，在cd后面跟>或>>再加上文件名就可以创建一个内容为空的文件。它和echo的区别之处在于echo可写文件内容，而cd并不能



## head命令

### 显示文件的前5行（两种方式）

```bash
head -n 5 test.txt
head -5 test.txt
```

### 显示文件的前100个字符

```bash
head -c 100 test.txt
```

### 显示文件的第10-20行

```bash
head -20 test.txt | tail -10
```



## cat命令

### 获取test.txt文件所有内容

```bash
cat test.txt
```

### 无论是否为空行，都显示行号

```bash
cat -n test.txt
```

### 显示行号，除了空行

```bash
cat -b test.txt
```

### 连续读取两个文件，按顺序输出

```bash
cat test1.txt test2.txt
```

### 倒序输出

cat倒过来写即可

```bash
tac test.txt
```



## nl命令

### 显示行号，除了空行

默认就是这个

```bash
nl test.txt
nl -b t test.txt 
```

### 无论是否为空行，都显示行号

```bash
nl -b a test.txt
```

### 行号靠最左显示

```bash
nl -n ln test.txt
```

![image-20210722164919771](https://img.rockche.cn//image-20210722164919771.png)

### 行号靠最右显示

```bash
nl -n rn test.txt
```

![image-20210722164946206](https://img.rockche.cn//image-20210722164946206.png) 

### 行号靠最右显示，不足位数左边补0

```bash
nl -n rz test.txt
```

![image-20210722165017324](https://img.rockche.cn//image-20210722165017324.png)

Linux 查看端口占用情况可以使用 **lsof** 和 **netstat** 命令。

------

## lsof命令

lsof(list open files)是一个列出当前系统打开文件的工具。

lsof 查看端口占用语法格式：

```
lsof -i:端口号
```

### 实例

查看服务器 8000 端口的占用情况：

```
# lsof -i:8000
COMMAND   PID USER   FD   TYPE   DEVICE SIZE/OFF NODE NAME
nodejs  26993 root   10u  IPv4 37999514      0t0  TCP *:8000 (LISTEN)
```

可以看到 8000 端口已经被轻 nodejs 服务占用。

lsof -i 需要 root 用户的权限来执行，如下图：

![image-20210827172125586](https://img.rockche.cn//image-20210827172125586.png)

更多 lsof 的命令如下：

```
lsof -i:8080：查看8080端口占用
lsof abc.txt：显示开启文件abc.txt的进程
lsof -c abc：显示abc进程现在打开的文件
lsof -c -p 1234：列出进程号为1234的进程所打开的文件
lsof -g gid：显示归属gid的进程情况
lsof +d /usr/local/：显示目录下被进程开启的文件
lsof +D /usr/local/：同上，但是会搜索目录下的目录，时间较长
lsof -d 4：显示使用fd为4的进程
lsof -i -U：显示所有打开的端口和UNIX domain文件
```

------

### netstat

**netstat -tunlp** 用于显示 tcp，udp 的端口和进程等相关情况。

netstat 查看端口占用语法格式：

```
netstat -tunlp | grep 端口号
```

- -t (tcp) 仅显示tcp相关选项
- -u (udp)仅显示udp相关选项
- -n 拒绝显示别名，能显示数字的全部转化为数字
- -l 仅列出在Listen(监听)的服务状态
- -p 显示建立相关链接的程序名

例如查看 8000 端口的情况，使用以下命令：

```
# netstat -tunlp | grep 8000
tcp        0      0 0.0.0.0:8000            0.0.0.0:*               LISTEN      26993/nodejs   
```

更多命令：

```
netstat -ntlp   //查看当前所有tcp端口
netstat -ntulp | grep 80   //查看所有80端口使用情况
netstat -ntulp | grep 3306   //查看所有3306端口使用情况
```

------

### kill

在查到端口占用的进程后，如果你要杀掉对应的进程可以使用 kill 命令：

```
kill -9 PID
```

如上实例，我们看到 8000 端口对应的 PID 为 26993，使用以下命令杀死进程：

```
kill -9 26993
```
