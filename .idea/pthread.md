# 进程和线程

## 进程

进程（Process）是linux系统的基本调度单位。

进程的概念首先是在60年代初期由MIT的Multics系统和IBM的TSS/360系统引入的。

经过了40多年的发展，人们对进程有过各种各样的定义。现列举较为著名的几种。 

（1）进程是一个独立的**可调度**的活动（E. Cohen，D. Jofferson） 

（2）进程是一个抽象实体，当它执行某个任务时，将要分配和释放各种资源（P. Denning） 

（3）进程是可以**并行执行**的计算部分。（S. E. Madnick，J. T. Donovan） ，看着是并行执行的，其实是哪个着急先运行哪个

以上进程的概念都不相同，但其本质是一样的。它指出了进程是一个程序的一次执行的过程。

**注**：进程和程序的区别：

**程序**是**静态**的，它是一些保存在磁盘上的指令的有序集合(2进制)，没有任何执行的概念；

**进程**是一个**动态**的概念，它是**程序执行的过程**，包括了**动态创建**、**调度**和**消亡**的整个过程。



**进程控制块**：pcb（包括进程的描述信息：进程号；**控制信息**：此时的状态；**资源信息**：占了多少内存；他是进程的一个静态描述）

在Linux中，进程控制块中的每一项都是一个**task_struct ** **结构**，它是include/linux/sched.h中定义的。



PID：进程号，唯一的；非零正整数

PPID：父进程号：非零正整数



#include<unistd.h>；进程头文件

```c
#include<stdio.h> 
#include<unistd.h> 
#include <stdlib.h> 
int main() 
{ 
/*获得当前进程的进程ID和其父进程ID*/ 
    printf("The PID of this process is %d\n",getpid()); 
    printf("The PPID of this process is %d\n",getppid());
    //while（1）；查看进程在哪
} 

```

运行的时候：输入gcc命令之后，输入./process：在前台启动；输入./process &：在后台启动

调度启动：系统根据用户的设置**自行**启动进程。

查看进程：ps aux|grep process

终止进程：kill -9+j进程号

​		：killall+进程名：结束所有的进程

ctrl+c：不是结束进程，而是强制中断进程，只能中断前台的进程

ctrl+z：将**任务中断**,但是此任务并没有结束,他仍然在进程中，只是维持挂起的状态，用户可以使用fg/bg操作继续前台或后台的任务，即进入睡眠状态

kill——通过向进程发送指定的信号——单进程

哪些信号？

| HUP  | 1    | 终端断线          |
| ---- | ---- | ----------------- |
| INT  | 2    | 中断（同 Ctrl+C） |
| QUIT | 3    | 退出（同 Ctrl+\） |
| TERM | 15     |  终止——**先结束相关程序**                 |
|   KILL   |  9    |    强制终止——**不考虑后果**               |
|   CONT   |   18   |    继续（与STOP相反， fg/bg命令）               |
|     STOP  | 19   |  暂停（同 Ctrl+Z）    |




**用户模式** **：**用户程序、应用程序或者内核之外的系统程序；给用户用

**内核模式**：给系统使用；在用户程序执行过程中出现**系统调用**或者发生**中断事件**，那么就要运行操作系统（即核心）程序，进程模式就变成内核模式。

在内核模式下运行的进程可以执行机器的特权指令，而且此时该进程的运行不受用户的干扰，即使是root用户也不能干扰内核模式下进程的运行。 相当于是拥有最高权限

**fork函数**

1）fork() ——在Linux中创建一个新进程的**唯一**方法

**特别地**，它执行一次**却返回两个值**。

1--fork函数说明 

fork函数用于从已存在进程（**父进程**）中创建一个新进程（**子进程**）。

这两个分别带回它们各自的返回值，其中父进程的返回值是子进程的进程号，而子进程则返回0。因此，可以通过返回值来判定该进程是父进程还是子进程。 

使用fork函数得到的子进程是父进程的一个**复制品**，它从父进程处**继承了**整个进程的地。

址空间，包括进程上下文、进程堆栈、内存信息、打开的文件描述符、信号控制设定、进程优先级、进程组号、当前工作目录、根目录、资源限制、控制终端等，而子进程所**独有的只有它的进程号、资源使用和计时器等**。因此可以看出，使用fork函数的代价是很大的，**它复制了父进程中的代码段、数据段和堆栈段里的大部分内容**，使得fork函数的**执行速度并不很快**。

不使用if   else；会无限循环 

```c
\#include <sys/types.h> //  提供类型pid_t的定义

\#include <unistd.h> 

pid_t fork(void) 

0：子进程 

子进程ID（大于0的整数）：父进程 

-1：出错 
```

**举例**

```c
#include<stdio.h> 
#include<unistd.h> 
#include <stdlib.h> 
int main() 
{ 
/*获得当前进程的进程ID和其父进程ID*/ 

	int a =0;
	a = fork();
 	printf("The PID of this process is %d\n",getpid()); 
 	printf("The PPID of this process is %d\n",getppid());
	
	if(-1==a)
	{
		printf("error");
	}
	printf("pid:%d\r\n",a);
	
	while(0);
	return 0;

} 
```

```c
root@hejin-desktop:/home/helloworld# ./hehe
The PID of this process is 2538
The PPID of this process is 2005
pid:2539
root@hejin-desktop:/home/helloworld# The PID of this process is 2539
The PPID of this process is 1
pid:0
//之所以root在第二个pid：0之前显示是因为，root比他先运行了一下
    因为fork()函数，复制父进程./hehe；所以程序运行了两次
```

**vfork()**

只复制父进程有用的，了解就好

**wait()**

**同步**

1）wait——等待子进程中断或结束  **阻塞**(就像getch()函数)

1--说明 

分析是否当前进程的某个子进程已经退出

若找到了这样一个已经变成僵尸的子进程，wait就会收集这个子进程的信息

2--语法

  **头文件**： #include<sys/types.h>
   \#include<sys/wait.h>

  **原型**： pid_t wait (int * status);
   **参数说明**：如果参数status的值不是NULL，wait就会把子进程退出时的状态取出并存入

  **常用**： 

WIFEXITED(status)，子进程是否为正常退出的，如果是，它会返回一个非零值

WEXITSTATUS(status) ，提取子进程的返回值

  返回值：等到的子进程号

3—使用说明

一旦等到任意子进程结束，即结束

**waitpid()**

**头文件**： #include<sys/types.h>
   \#include<sys/wait.h>

  **原型**： pid_t waitpid(pid_t pid,int * status,int options);
    **参数说明**： 

**pid**:

pid>0时，只等待进程ID等于pid的子进程，不管其它已经有多少子进程运行结束退出了，只要指定的子进程还没有结束，waitpid就会一直等下去。

pid=-1时，等待任何一个子进程退出，没有任何限制，此时waitpid和wait的作用一模一样。

pid=0时，等待同一个进程组中的任何子进程，如果子进程已经加入了别的进程组，waitpid不会对它做任何理睬。

pid<-1时，等待一个指定进程组中的任何子进程，这个进程组的ID等于pid的绝对值。

**status**

  同wait

**options——**一个或多个标志符按位“或”的结果，额外的选项控制pid

  **0****：无额外控制**

   WNOHANG：即使没有子进程退出，它也会立即返回，不像wait那样永远等下去

   WUNTRACED ：感兴趣自行了解

  **返回值**：

A.当正常返回的时候，waitpid返回收集到的子进程的进程ID；

B.如果设置了选项WNOHANG，而调用中waitpid发现没有已退出的子进程可收集，则返回0；

C.如果调用中出错，则返回-1，这时errno会被设置成相应的值以指示错误所在

**注**：wait与waitpid

static inline pid_t wait(int * wait_stat)

{

return waitpid(-1,wait_stat,0);

}

**终止（退出）**

1）正常退出

1—main函数执行结束（return）——执行完后把控制权交给调用函数

2—exit——执行完后把控制权交给OS

系统无条件的停止剩下所有操作，清除包括PCB在内的各种数据结构，并终止本进程的运行。

3--_exit——执行后立即返回给内核

2）异常退出

1--收到某个信号

2—about

## 线程

**线程**（英语：thread）是进程内的基本调度单位

线程不是独立的，是共享的

```c
//头文件只是为了声明
#include<stdio.h> 
#include<pthread.h>

void* pthread1(void *a)
{
printf("hello");
return NULL;
}
int main() 
{ 
//创建一个函数
	pthread_t id=0;	
	if(0!=pthread_create(&id,NULL,pthread1,NULL))
	{
		printf("pthread_creat failed.\r\n");	
		return 0;
	}
	printf("hello1");
	if(0!=pthread_join(id,NULL))		//为了等待id，也就是pthread1这个函数执行
	{
		printf("pthread_join failed.\r\n");
	}
	return 0;
}

因为是线程是共享的，所以hello,hello1,main函数谁都有可能先结束，所以要用join函数等待。
```

```c
root@hejin-desktop:/home/helloworld# gcc -o ./thread ./thread.c -lpthread
//-l:-link     pthread:库，意思就是链接库
```



# 总结

## 一、进程

（一）、基本概念

1. 什么是进程

   - 相互独立的，动态的
   - Linux系统中的基本调度单位
2. 进程和程序的区别

   * 进程：动态	程序：静态
3. PCB：进程控制块
   * 描述某一时刻和进程相关信息的
4. 进程的唯一标志（句柄）：pid     ppid（父进程号）int getpid(void)  ;int getppid(void)

 （二）、进程活动过程

1. 创建：

   * 用户：通过命令启动

        * 前台启动：运行可执行程序

        * 后台启动：在命令后加 & （进程需要长期运行）

   * 系统：调度启动
   * 开发者：
     * int fork(void);	//void 无类型
     * 返回：进程号（2个）
     * fork的本质：复制父进程（代码段、内存段、堆栈段……），复制fork之后的内容，但是进程号是唯一的
     * vfork 和 fork的区别：vfork只复制vfork函数之后有用的

2. 调度

   * 用户：查看进程信息：ps aux|grep 进程名		需要知道 进程号，进程名，进程状态

   * 进程状态：

        | R (TASK_RUNNING)                | 可执行状态&运行状态(在run_queue队列里的状态)                 |
        | ------------------------------- | ------------------------------------------------------------ |
        | S (TASK_INTERRUPTIBLE)          | 可中断的睡眠状态，**可处理 ** signal                         |
        | D (TASK_UNINTERRUPTIBLE)，      | 不可中断的睡眠状态,**可处理**  signal,　**有延迟**           |
        | T (TASK_STOPPED or TASK_TRACED) | 暂停状态或跟踪状态,不可处理signal,因为根本没有时间片运行代码 |
        | Z (TASK_DEAD - EXIT_ZOMBIE)，   | 退出状态，进程成为僵尸进程。不可被kill,即不响应任务信号,　无法用SIGKILL杀死 |

   * 系统调度

   * 开发者：

        进程同步：wait  ；  waitpid

3. 消亡

   * 用户：
     * ctrl+c：中断进程
     * ctrl+z：暂停
     * kill+9+pid    killall+进程名：不考虑后果结束

   * 系统消亡

   * 开发者

     * 顺其自然：main  最后一行 return 0

       ​		：exit：交给操作系统

     * 强制：about

（三）其他

1. 守护进程：（脚本  c语言）创建
2. 僵尸进程（信息依旧存在进程表中 ，只有结束父进程）     孤儿进程：消亡
## 线程

（一）基本概念

1. 线程是进程的基本调度单位
2. 线程和进程的本质区别：资源方面：线程是共享的        进程是独立的

（二）线程的过程

**pthread_t：线程句柄类型，本质上是一个整型   任何一个线程都有线程ID，即句柄**

1. 创建												int test (char a)

   * pthread_create（pthread_t  * id，pthread_attr_t    ，void* (* p1)（void * ），void* arg）

      				句柄 		线程属性 NULL	函数类型  函数指针  函数入参（线程函数地址）

     第一个入参：获取id号

     void* 函数返回值不可以改变

2. 调度（使用）

   * int pthread_join(pthread_t  *id,void *);第二个入参通常为空

     阻塞函数：若等不到所等线程会一直等

     防止线程所共享的资源释放

3. 结束

   * 顺其自然
   * pthread_exit

（三）什么时候用线程

当若干个任务需同时进行时（并行时）

write_buf:是一个数组，所以他代表首地址

# 补充

线程函数里一般不写初始化的内容

sleep函数，一个入参，毫秒

1.基本完成一读一写

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <pthread.h>

int flag = 1;

int file_size(FILE *fp);

void * p_fread(void *a)
{
	FILE *fp  =(FILE *)a;
	int size = 0 ,size_last = 0,ch = 0;
	printf("a:%p fp:%p\r\n",a,fp);
	while(1)
	{	
		if (!flag)
//		size = file_size(fp);
//		if((size-size_last)>0)
		{
			if(fseek(fp,-1,SEEK_CUR))
			{
				printf("fseek failed.\r\n");
				return 0;
			}
			ch=fgetc(fp);
		        printf("a_read:%c\r\n",ch);
			flag=1;
		}

//		size_last= size;
	}
	return 0;
}
//根据文件大小确定是先进行的main函数里的write先运行，然后才是线程里的内容，所以用file_size 的fseek判断可不可以继续执行下面的读
/*int  file_size(FILE* fp)
{
	int size=0;	

	if(fseek(fp,0,SEEK_END))
	{
		printf("fseek failed.\r\n");
		return 0;
	}
	size=ftell(fp);	
	while(0)
	{//EOF:文件结束符	
		if(EOF!=fgetc(fp))
		{
			size++;//计个数
		}	//读完以后，内部指针会自动往后移,每次只读一个字符
		else break;
	}
	return size;
}
*/

int main(int argc,char** argv)
{
	FILE *fp=NULL;
	char write_buf[10]="hello";
	char read_buf[10]={0};
	size_t w_count=0;
	pthread_t id_read=0;	
	int i=0,s=0,pcr=0;
		
	fp=fopen("./name1","w+");
	if(fp==NULL)
	{
		printf("fopen failed.\r\n");
		return 0;	
	}
	if(-1==pthread_create(&id_read,NULL,p_fread,fp))
	{
		printf("pthread_creat failed.\r\n");	
		return 0;
	}

	while(*(write_buf+i))
	{	
		if(flag)
		{
			w_count=fwrite(write_buf+i,1,1,fp);  //write_buf：是一个数组，所以他的名字就是他的首地址
			if(w_count==0)
			{
				printf("fwrite failed.\r\n");
				return 0;
			}
			printf("a_write:%c\r\n",*(write_buf+i));
			i++;
			flag=0;	
		}	
	}
	printf("file_size:%d\r\n",file_size(fp));
	if(0!=pthread_join(id_read,NULL))
	{
		printf("pthread_join failed.\r\n");
	}	
	
//fread之后，在文件中的光标继续呆在fseek设置的那个位置，依情况而定
/*	w_count=fwrite(read_buf,strlen(read_buf),1,fp);
	if(w_count==0)
	{
		printf("fwrite failed.\r\n");
		return 0;
	}	
    //只写了没有读所以不用seek
	
	fclose(fp2);*/
fclose(fp);
}
```

