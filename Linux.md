# Linux命令

查看Ubantu版本号

```bash
lsb_release -a
```

查看安装的软件

```bash
rpm -qa
```

 退出登录服务器

```
ctrl+d
```

查看当前ip

```shell
ifconfig
# or
ifconfig en0
```

查看ip是否跑通

```shell
ping ip地址
```

#### 1. 文件与目录管理

```shell
# 查看路径
pwd  # 查看当前位置的全路径
# 切换路径
cd /tmp/movie # 切换到temp目录下面的movie目录
cd .. # 切换到当前目录的上一级目录
# 复制
cp [原文件] [目标目录] # 复制文件
cp /root/install.log /tmp # 把root目录下的install.log文件复制到tmp目录下
cp -r /tmp/movie /root # 如果想复制整个目录，需要加一个选项：-r。  把tmp目录下的movie目录复制到root目录下
cp -r /tmp/book /tmp/image /root # 同时复制多个目录或文件。把tmp目录下的book目录和image目录同时复制到root目录下
# 删除
rm /tmp/abc.txt # 删除tmp目录下的abc.txt文件
rm -r /tmp/movie # 删除目录加一个-r，删除tmp目录下的movie目录
rm `ls | grep -v "xgb"` # 除了含有xgb字段的文件，删除其余文件，注意是反引号
# 创建目录
mkdir /tmp/movie # 在tmp目录下创建一个名为movie的目录，其中 /tmp/ 是路径，movie 是你自己起的目录名
mkdir -p /tmp/book/programming # 创建一个多级目录，即使book目录原本不存在，也可以创建成功
mkdir /tmp/image /tmp/music # 创建多个目录，在tmp目录下同时创建image和music两个子目录
# 移动或重命名
mv，介绍的 cp 命令的用法，mv 命令也同样适用，只不过 cp 是复制文件或目录，mv 命令是移动。如果你把当前目录下的一个文件或目录移动到当前目录，同时改名，那么就相当于给这个目录或文件重命名了。
```

#### 2. 查看进程

不挂断的运行命令

注意：在后台执行看不到运行的情况，报错也是看不到的。

若不挂断运行命令，如果进程PID查不到，则表示命令执行结束

```bash
nohup Command [ Arg … ] &
# &：让命令在后台执行，终端退出后命令仍旧执行。
# nohup:No hung up，即不挂断
# 无论是否将 nohup 命令的输出重定向到终端，输出都将附加到当前目录的 nohup.out 文件中。
# 如果当前目录的 nohup.out 文件不可写，输出重定向到 $HOME/nohup.out 文件中。
# 如果没有文件能创建或打开以用于追加，那么 Command 参数指定的命令不可调用。
```

查看运行的后台进程

```bash
jobs -l  #只能查看当前终端的生效的进程，若关闭该终端，在另一个终端无法看到这个终端生效的进程
```

```bash
ps -aux | grep "runoob.sh"  #查找到 nohup 运行脚的PID(进程号)，
# a : 显示所有程序
# u : 以用户为主的格式来显示
# x : 显示所有程序，不区分终端机

ps -def | grep "runoob.sh"
# ps：进程查看命令
# 中间的|是管道命令 是指ps命令与grep同时执行
# grep(Global Regular Expression Print)命令是查找，是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出 来，它的使用权限是所有用户。

ps -aux | grep "houmin" #查找用户名为“houmin”运行的PID
```

Head标头含义

```
USER 用户名
UID 用户ID（User ID）
PID 进程ID（Process ID）
PPID 父进程的进程ID（Parent Process id）
SID 会话ID（Session id）
%CPU 进程的cpu占用率
%MEM 进程的内存占用率
VSZ 进程所使用的虚存的大小（Virtual Size）
RSS 进程使用的驻留集大小或者是实际内存的大小，Kbytes字节。
TTY 与进程关联的终端（tty）
STAT 进程的状态：进程状态使用字符表示的（STAT的状态码）
  （1）状态：
  R 运行 Runnable (on run queue) 正在运行或在运行队列中等待。
  S 睡眠 Sleeping 休眠中, 受阻, 在等待某个条件的形成或接受到信号。
  I 空闲 Idle
  Z 僵死 Zombie（a defunct process) 进程已终止, 但进程描述符存在, 直到父进程调用wait4()系统调用后释放。
  D 不可中断 Uninterruptible sleep (ususally IO) 收到信号不唤醒和不可运行, 进程必须等待直到有中断发生。
  T 终止 Terminate 进程收到SIGSTOP, SIGSTP, SIGTIN, SIGTOU信号后停止运行运行。
  P 等待交换页
  W 无驻留页 has no resident pages 没有足够的记忆体分页可分配。
  X 死掉的进程
  （2）状态后缀：
  < 高优先级进程 高优先序的进程
  N 低优先 级进程 低优先序的进程
  L 内存锁页 Lock 有记忆体分页分配并缩在记忆体内
  s 进程的领导者（在它之下有子进程）；
  l 多进程的（使用 CLONE_THREAD, 类似 NPTL pthreads）
  + 位于后台的进程组
START 进程启动时间和日期
TIME 进程使用的总cpu时间
COMMAND 正在执行的命令行命令
NI 优先级(Nice)
PRI 进程优先级编号(Priority)
WCHAN 进程正在睡眠的内核函数名称；该函数的名称是从/root/system.map文件中获得的。
FLAGS 与进程相关的数字标识
```

杀死进程

```bash
kill [PID]  # 安全的干净的杀死进程，发出默认信号SIGTERM（15）即 kill -15
# 程序在接收到信号后，可以自己作出选择，1、立即停止程序 2、释放响应资源后停止程序 3、忽略该信号，继续执行程序，所以程序有时候不会立即终止或者拒绝终止
kill -9 [PID]  # 强硬杀死进程，发出信号为SIGKILL，要求接收到该信号的程序应该立即结束运行，不能被阻塞或者忽略，可能造成数据丢失或数据终端无法恢复正常状态。
```

#### 3. shell相关

```shell
# zsh与bash之间切换
chsh -s /bin/bash  # 切换为bash
chsh -s /bin/zsh   # 切换为zsh
# 记得输入切换命令后，要重新打开终端terminal才生效哦！ 大功告成！

# 查看当前shell，但不能时时反映shell，需重启iTerm2
echo $SHELL

# 执行脚本，脚本最后可以添加这一句，表示脚本运行完成
echo "The script is finished"
```

光标移动快捷键

- 将光标移动到行首：control + a
- 将光标移动到行尾：control + e
- 清除屏幕：control + l
- 搜索以前使用命令：control + r
- 清除当前行：control + u
- 清除至当前行尾：control + k
- 单词为单位移动：option + 方向键
- 放弃当前输入：control + z

#### 4.内存查看及管理

##### A. 查看磁盘空间

```shell
df -h  # 查看磁盘各分区大小，已用空间大小
du -sh foo  # 查看foo目录的大小
du -sh * # 查看当前目录下的搜索文件和子目录大小
# 用df查看U盘或磁盘挂载的path，然后用umount path，来卸载U盘
```

##### B. 查看系统内存使用情况

（1）free：显示系统内存使用情况，包括物理内存、交换内存、内核缓冲区内存

free 命令中的信息都来自于 /proc/meminfo 文件

```shell
free -h　　　　　　//以更友好的方式显示，会以K、M、G为单位来显示
free -h -s 3　　 //以一定时间间隔重复的输出，这个命令是每3秒输出一次
```

```shell
$ free
              total        used        free      shared  buff/cache   available
Mem:       32946324     2489392    11422656     1622872    19034276    28352888
Swap:             0           0           0
```

Mem：内存使用情况。

Swap：交换空间（虚拟内存）使用情况。

total：系统总共可用物理内存、交换空间大小。

used：已经被使用的物理内存、交换空间大小。

free：剩余可用物理内存、交换空间大小。

shared：被共享使用的物理内存大小。

buff/cache：被 buffer 和 cache 使用的物理内存大小。

available：还可以被应用程序使用的物理内存大小。

（2）vmstat（virtual meomory statistic）：对操作系统的虚拟内存、进程、CPU活动进行监控，是对系统的整体情况进行的统计

```shell
vmstat 1　　　　//每隔1s打印一次
vmstat 1 5　　　//每隔1秒打印一次，打印五次
vmstat -s　　　　//显示内存相关统计信息及多种系统活动数量
```

```
$ vmstat
procs   -----------memory-----------  --swap--  --io--  --system--  -----cpu-----
 r  b   swpd    free   buff   cache   si   so   bi  bo   in   cs    us sy id wa st
 1  0     0  14376368 161976 1130836   0    0    0   3    2    2     0  0 100 0  0
```

- memory列
  - swpd：使用的虚拟内存大小。
  - free：空闲物理内存大小。
  - buff：buffer cache内存大小。
  - cache：page cache的内存大小。

- swap列
  - si：每秒从交换区读入到内存的大小，由磁盘调入内存（单位：kb/s）
  - so：每秒从内存写出到交换区的大小，由内存调入磁盘（单位：kb/s）

（3）top

使用top命令，可以查看正在运行的进程和系统负载信息，包括cpu负载、内存使用、各个进程所占系统资源等，top命令以一定频率动态更新这些统计信息

##### C. 清理缓存

```shell
sync  # 为了防止内容丢失,运行sync会把未存盘的cache都写入磁盘，稍等片刻, 或者是直接运行sync两遍
echo 3 > /proc/sys/vm/drop_caches
>>-bash: /proc/sys/vm/drop_caches: Permission denied
sudo -s
>>
echo 3 > /proc/sys/vm/drop_caches
# 注意：清缓存前记得加sync,多执行几遍…防止丢失

# 操作说明
echo 1 > /proc/sys/vm/drop_caches  # 表示清除pagecache
echo 2 > /proc/sys/vm/drop_caches  # 表示清除回收slab分配器中的对象（包括目录项缓存和inode缓存）。slab分配器是内核中管理内存的一种机制，其中很多缓存数据实现都是用的pagecache。
echo 3 > /proc/sys/vm/drop_caches  # 表示清除pagecache和slab分配器中的缓存对象
```

####  6.cat命令

文本输出命令，三大功能：

- 一次性显示整个文件

```shell
cat filename
```

- 创建一个文件

```shell
cat	> filename
```

- 将几个文件合并为一个新的文件

```shell
cat file1 file2 > file  # 没有>则输出到屏幕
```

参数：
-n 或 –number 由 1 开始对所有输出的行数编号
-b 或 –number-nonblank 和 -n 相似，只不过对于空白行不编号
-s 或 –squeeze-blank 当遇到有连续两行以上的空白行，就代换为一行的空白行

```shell
# 把 linuxfile1 的档案内容加上行号后输入 linuxfile2 这个档案里
cat -n linuxfile1 > linuxfile2
# 把 linuxfile1 和 linuxfile2 的档案内容加上行号(空白行不加)之后将内容附加到 linuxfile3 里。
cat -b linuxfile1 linuxfile2 >> linuxfile3
# 此为清空/etc/test.txt档案内容
cat /dev/null > /etc/test.txt 
```



#### 5.vi/vim命令

```shell
vi或vim 文件名  #打开文件并进入文件的查看和编辑模式
```

```bash
esc #进入命令模式
```

相关命令

```bash
i #键入模式
:q #退出
:q! #强制退出
:wq #写入保存退出
:wq! #写入保存强制退出
gg dG #全选删除
gg #移动到档案的第一行
G  #移动到档案的最后一行
```

添加/删除注释

```
cltr+v 进入visual block，移动光标选中要注释的行的开头，按下I进入insert，插入注释符，如#，按下esc完成注释添加
cltr+v 进入visual block，移动光标选中要删除注释的行的开头，按下d删除注释，按下esc完成注释删除
```



## Linux中的相关配置

#### 1. jupyter-notebook配置文件

```bash
jupyter notebook --generate-config # 新建jupyter配置文件
vim ~/.jupyter/jupyter_notebook_config.py  # 配置配置文件
```

```bash
import os
from IPython.lib import passwd

# mixinIP = '0.0.0.0'
# if 'IP' in os.environ:
#   mixinIP = os.environ['IP']
# c.ConnectionFileMixin.ip = mixinIP
c.NotebookApp.ip = '10.186.4.36'
c.NotebookApp.port = int(os.getenv('JUPYTER_PORT', 8048))
c.NotebookApp.open_browser = False
c.MultiKernelManager.default_kernel_name = 'python3'

# set a password if PAS is set in the environment
pw = '96338522456klm'
if 'PASS' in os.environ:
  pw = os.environ['PASS']
c.NotebookApp.password = passwd(pw)
# del os.environ['PASS']
```

普通启动jupyter-lab

```bash
jupyter-lab
cltr+c #退出jupyter
```

后台运行jupyter，关掉浏览器仍不会停止，需要在进程中kill

```bash
nohup jupyter-lab &  
```

#### 2. bash的配置文件

```shell
~/.bash_profile   # 个人配置文件
# 更改后
source ~/.bash_profile # 使得刚才编辑内容生效
```



## Tumx使用

#### 1. 什么是Tumx

用户与计算机的临时交互称为一次会话

Tumx将会话与窗口解绑，将他们彻底分离，终端窗口关闭，会话不会终止，其中的进程继续运行，需要时在让会话连接其他窗口

```
（1）它允许在单个窗口中，同时访问多个会话。这对于同时运行多个命令行程序很有用。

（2） 它可以让新窗口"接入"已经存在的会话。

（3）它允许每个会话有多个连接窗口，因此可以多人实时共享会话。

（4）它还支持窗口任意的垂直和水平拆分。
```

安装

```bash
# Ubuntu 或 Debian
$ sudo apt-get install tmux

# CentOS 或 Fedora
$ sudo yum install tmux

# Mac
$ brew install tmux
```

#### 2. 基本用法

```bash
tmux  #启动，底部状态栏左侧是窗口编号和名称，右侧是系统信息
cltr+d 或 显示输入 exit  # 退出
cltr+b  # 前缀键，唤醒快捷键  （通过esc键或q键退出）
```

#### 3. 会话管理

新建会话：第一个启动的窗口编号为0，对应的会话为0号会话，以此类推

```bash
# 也可以给窗口命名来区分窗口
$ tmux new -s <session-name>
```

分离会话：将当前会话与窗口分离，退出当前 Tmux 窗口，但是会话和里面的进程仍然在后台运行

```bash
按下cltr+b d
# or
$ tmux detach
```

```bash
# 查看当前的所有会话
$ tmux ls
# or
$ tmux list-session
```

接入会话：重新接入已经存在的会话

```bash
# 使用会话编号
$ tmux attach -t 0
# 使用会话名称
$ tmux attach -t <session-name>
```

切换会话

```bash
# 使用会话编号
$ tmux switch -t 0
# 使用会话名称
$ tmux switch -t <session-name>
```

杀死会话：杀死某个会话

```bash
# 使用会话编号
$ tmux kill-session -t 0
# 使用会话名称
$ tmux kill-session -t <session-name>
```

重命名会话

```bash
$ tmux rename-session -t 0 <new-name>
```

相关快捷键

```bash
Ctrl+b d：分离当前会话。
Ctrl+b s：列出所有会话。
Ctrl+b $：重命名当前会话。
```

#### 4. 窗格操作

划分操作

```bash
# 划分上下两个窗格
$ tmux split-window
# 划分左右两个窗格
$ tmux split-window -h
```

移动光标

```bash
# 光标切换到上方窗格
$ tmux select-pane -U
# 光标切换到下方窗格
$ tmux select-pane -D
# 光标切换到左边窗格
$ tmux select-pane -L
# 光标切换到右边窗格
$ tmux select-pane -R
```

交换窗格位置

```bash
# 当前窗格上移
$ tmux swap-pane -U
# 当前窗格下移
$ tmux swap-pane -D
```

相关快捷键

```
Ctrl+b %：划分左右两个窗格。
Ctrl+b "：划分上下两个窗格。
Ctrl+b <arrow key>：光标切换到其他窗格。<arrow key>是指向要切换到的窗格的方向键，比如切换到下方窗格，就按方向键↓。
Ctrl+b ;：光标切换到上一个窗格。
Ctrl+b o：光标切换到下一个窗格。
Ctrl+b {：当前窗格与上一个窗格交换位置。
Ctrl+b }：当前窗格与下一个窗格交换位置。
Ctrl+b Ctrl+o：所有窗格向前移动一个位置，第一个窗格变成最后一个窗格。
Ctrl+b Alt+o：所有窗格向后移动一个位置，最后一个窗格变成第一个窗格。
Ctrl+b x：关闭当前窗格。
Ctrl+b !：将当前窗格拆分为一个独立窗口。
Ctrl+b z：当前窗格全屏显示，再使用一次会变回原来大小。
Ctrl+b Ctrl+<arrow key>：按箭头方向调整窗格大小。
Ctrl+b q：显示窗格编号。
```

#### 5. 窗口管理

新建窗口

```
$ tmux new-window
# 新建一个指定名称的窗口
$ tmux new-window -n <window-name>
```

切换窗口

```
# 切换到指定编号的窗口
$ tmux select-window -t <window-number>
# 切换到指定名称的窗口
$ tmux select-window -t <window-name>
```

给当前窗口重命名

```
$ tmux rename-window <new-name>
```

相关快捷键

```
Ctrl+b c：创建一个新窗口，状态栏会显示多个窗口的信息。
Ctrl+b p：切换到上一个窗口（按照状态栏上的顺序）。
Ctrl+b n：切换到下一个窗口。
Ctrl+b <number>：切换到指定编号的窗口，其中的<number>是状态栏上的窗口编号。
Ctrl+b w：从列表中选择窗口。
Ctrl+b ,：窗口重命名。
```

#### 6. 其他命令

```bash
# 列出所有快捷键，及其对应的 Tmux 命令
$ tmux list-keys
# 列出所有 Tmux 命令及其参数
$ tmux list-commands
# 列出当前所有 Tmux 会话的信息
$ tmux info
# 重新加载当前的 Tmux 配置
$ tmux source-file ~/.tmux.conf
```

#### 7.iterm2和oh-my-zsh的安装与配置

https://www.jianshu.com/p/246b844f4449



## 文件中转sfpt

<img src="https://img2018.cnblogs.com/blog/801822/201810/801822-20181031164035000-1733068832.png" style="zoom: 67%;" />

```
help # 查看相关命令
pwd  # 查看远程机当前路径
lpwd # 查看本地机当前路径
cd   # 转到远程机某路径
lcd  # 转到本地机某路径
bye或exit #退出sftp
```

1. 将服务上的代码转到本地

先在服务器上put到sftp，再到本地上启动shell从sftp中get文件到本地

2. 将本地代码上传到服务器

现在本地上put文件到sftp，再到服务器上启动shell从sftp中get文件到服务器的文件夹

```
上传和下载文件夹
sftp> mkdir Tecmint  # 先在远程服务器创建该文件夹
sftp> put -r Tecmint # 再进行上传，要加-r
sftp> get -r Tecmint # 下载该文件夹到本地，要加-r
```

```
删除
rm  # 删除文件
rmdir  # 只能删除空文件夹，要先rm folder/*
```



