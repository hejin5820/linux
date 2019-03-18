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

touch: 创建文件

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

安装命令：apt -get install net-tools 

本地回环：127.0.0.1    点分十进制   最后一位主机位，同一局域网内ip前三位一样



ping 8.8.8.8(谷歌的地址)

ctrl+c：结束当前进程

ctrl+shift+c：复制

ctrl+shift+v：粘贴

windows下，右键就是粘贴



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

​	    











