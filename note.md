# 2019.3.15

虚拟机分类：VMware,Virtual Box,Virtual PC 

​			Linux虚拟机,JVM (java的虚拟机)

Host ( 主机/实体 )    Guest ( 宿主机 )

.net 开发[windows]()

# 命令

ctrl+alt+T：打开终端

cd：跳转到目录

/ ：根目录

. /：当前目录

.. /：上一级目录

创建文件：touch

ls：查看当前目录下的文件

ll： 详细的显示文件细节

界面：图形化界面，gedit

安装文件夹，mkdir

ls下的文件是白色就是没有权限

增加权限：chmod 777 文件名

cmd（命令）-help（使用说明）或者  -- help     --version（查版本）

【要填的参数】

clear：翻页



sudo passwd root：修改root密码

[sudo：获取系统的root权限][https://blog.csdn.net/Sacredness/article/details/81224671]

[ubuntu tools安装][https://jingyan.baidu.com/album/93f9803f0d9d9be0e46f55ce.html?picindex=5]

/root/.profile

sudo gedit 文件名

# 2019.3.18

本地回环：127.0.0.1    点分十进制   最后一位主机位，同一局域网内ip前三位一样



ping 8.8.8.8(谷歌的地址)

ctrl+c：结束当前进程

ctrl+shift+c：复制

ctrl+shift+v：粘贴

windows下，右键就是粘贴



安装命令：apt -get install net-tools 

[更换镜像文件内容gedit /etc/apt/sources.list][https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/]

[apt-get update][https://blog.csdn.net/weixin_41762173/article/details/79480832]

查询ifconfig

安装 apt -get install net-tools 

然后就可以查询了



桥接模式：同一个局域网内，主机和虚拟机用的同一个网络，所以，主机，虚拟机，别的主机，外网之间都可以ping通。（相当于虚拟机也连了网线）

NAT模式：主机虚拟出一个网络供自己的虚拟机使用，只有自己的主机和自己的虚拟机可以ping通，也可以连接外网

仅主机模式：和NAT模式一样，但是不可以连接外网



## 编译

安装编辑c环境：apt-get install gcc

输出可执行文件：后缀名是无用的，本质都是二进制

| 命令| 当前文件夹下新的文件 | 当前文件下的.c文件 |
| :-----: | :------------------: | :----------------: |
| gcc  -o | ./test               | ./main.c         |

./test：接下来就可以查看.c运行之后的内容



### samba安装

Ubuntu：只是一个为了使用gcc的一个环境

Samba可以实现在windows编写，可以同步到Ubuntu

1.  apt-get install samba
2. apt-get install smbfs

​         但是根据提示，apt-get install cifs-utils

3. 安装完成后执行 

   ​    samba -V

   如果可以看到版本号即为安装成功

4. system-config-samba:启动

   [启动出现问题][https://www.cnblogs.com/django816/p/10440397.html]

5. 查看进程，可以看smb是否在运行

   ps aux

   或者：ps aux|grep **smd**

# 2019.3.19  总结：

### 基本业内知识

* 平台
  * 虚拟机：什么  分类  主流
  * linux系统：什么  主流
* 软件版本号
* 网络
  * ip地址
  * 点分十进制
  * 主机位
  * 常用ip：127.0.0.1	8.8.8.8

### Linux环境搭建 

1. 虚拟机——VMware

   * 安装

   * 配置

     * 工具（系统装好且root）——VMware

       优点：屏幕自适应，与windows文件拖拽、鼠标移动等

       选择安装VMwaretools（CD）—>cp到根目录—>解压—>运行安装脚本（.pl）—>重启系统

      * 网络

        桥接;NAT;仅主机

        设置：虚拟机—>设置—>网络适配器

        ​           编辑—>虚拟网络编辑

     * 验证：

       通外网（上网，ping8.8.8.8  ；  其他人的主机/虚拟机）

       虚拟机和主机通不通（互ping，既能收也能发）

2. linux——Ubuntu

   * 安装

     * 虚拟内存和虚拟硬盘的设置

   * 配置

     * 设置最高权限root

       （1）、 改密码：passwd root

       （2）. 进root：百度

       改文件：/etc/gdm/..

3. 代码工具

   * 代码编辑器：Vim（命令行）   gedit（图形化）      geany（图形化且功能强大）
   * 代码编译器：gcc
   * 调试工具：Gdb

4. 网络工具

   * Samba：和windows共享文件

     ​		【安装samba服务器 	安装sambafs	启动system-config-samba】

     ​		【配置——samba服务器

     ​		（新建共享文件夹并设置权限，可读可写  所有人）

     ​		（安全模式设置为share）

     ​		（被共享的文件夹权限设置）】

5. 总结：

   * 环境搭建的基本流程：平台（硬件；OS）—>代码工具（代码编辑，编译，调试工具）

     —>通信工具（有线 【串口】；无线【网络工具 文件共享与传输】）

   * 安装软件

     方式：直接安装（图形化安装，命令行安装）

     ​	   间接安装（下载源码（官网，源）—>修改源码+配置文件—>编译源码—>安装—>系统配置）

     步骤;安装—>设置（对功能进行配置）—>掌握基本操作（快捷方式，工程的建立）

   * 网络工具

     * 服务端
     * 客户端  

###  Ubuntu基本使用

1. 命令

   * 系统管理

     * passwd+用户名
   * 进程

     * 查看进程：ps auxigrep +进程名
     * ctrl+c：控制进程
   * 文件/文件夹管理
     * 修改权限：chmod+777+对象（路径+文件/文件夹名）
     * 创建文件夹：mkdir+对象（路径+文件/文件夹名）
     * 创建文件：touch+对象
     * 查看：ls+对象	ll+对象
     * 访问：cd+对象
   * 软件管理：

     * apt-get+instal(安装)l/update(更新)/卸载+软件名称（可以是多个）
   * 网络

     * ifconfig
     * ping
   * 其他

     * -v   -h
     * / ：根目录	. /：当前目录       .. /：上一级目录
     * sudo:获取root权限
     * ctrl+shift+c：复制	ctrl+shift+v：粘贴
     * service+服务名称+动作【start/stop】
     * /etc/init.d/服务名称+动作【start/stop】

2. 命令行

   * 特点：自动补全【路径、命令】tab
   * 历史记录【上下键】

3. 根目录

   /etc：系统配置       /etc/apt

   /home













