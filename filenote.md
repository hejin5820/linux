## 文件操作

数据：增 删 改 查

文件：流

新建	打开		写	保存	删除		复制	关闭	……

### 2019.3.19文件基本操作

1. 流程

   * 新建/打开—>读写—>关闭

2. 

   * open/fopen

     * 区别：

       

       * | 级别不同： | 系统级：fopen | 内核级 ：open         |
         | :--------: | :-----------: | :-------------------: |
         | 移植性     | 可移植        | 只能在linux系统下调用 |
         |权限|不能指定要创建文件的权限|可以指定权限|
         |文件|普通文件|设备文件（内设和外设）（串口）|
         |使用|直接调用|分配一个缓冲区供文件进行I/O（提高执行效率）|

       * 内核级（各个系统本质相同 ，即内核相同）

       * Ubuntu写的fopen到windows可以，但是open不可以

     * 各自的

       **fopen：两个入参**

       FILE * fopen(const char * path, const char * mode);

       文件类型：file*（路径+文件，形态流）

       FILE*  ：句柄(文件唯一标志)，本质上是一个指针其实就是文件流，他是被typedef替换了名字，其实就是

       路径：相对路径和绝对路径

       ​		相对路径:可执行文件在哪，就创建在哪

       typedef struct

       {

       }FILE;

       ​	文件名称：char*

       ​	打开方式：char*

       mode 有下列几种**形态流**字符串:

| 字符串 | **说明**                                                     |
| ------------ | ------------------------------------------------------------ |
| r            | 以只读方式打开文件，**该文件必须存在**。                     |
| r+           | 以读/写方式打开文件，**该文件必须存在**。             |
| rb+          | 以读/写方式打开一个二进制文件，只允许读/写数据。             |
| rt+          | 以读/写方式打开一个文本文件，允许读和写。                    |
| w            | 打开只写文件，若文件存在则文件**长度清为零**，即该文件内容会消失;若文件不存在则创建该文件。 |
| w+           | 打开可读/写文件，若文件存在则文件**长度清为零**，即该文件内容会消失;若文件不存在则创建该文件。 |
| a            | 以**附加的方式**打开只写文件。若文件不存在，则会创建该文件;如果文件存在，则写入的数据会被加到文件尾后，即文件原先的内容会被保留([EOF](https://baike.so.com/doc/630967-667766.html) 符保留)。 |
| a+           | 以附加方式打开可读/写的文件。若文件不存在，则会创建该文件，如果文件存在，则写入的数据会被加到文件尾后，即文件原先的内容会被保留(EOF符不保留)。 |
| wb           | 以只写方式打开或新建一个二进制文件，只允许写数据。           |
| wb+          | 以读/写方式打开或新建一个二进制文件，允许读和写。            |
| wt+          | 以读/写方式打开或新建一个文本文件，允许读和写。              |
| at+          | 以读/写方式打开一个文本文件，允许读或在文本末追加数据。      |
| ab+          | 以读/写方式打开一个二进制文件，允许读或在文件末追加数据。    |

官方;man

头文件：<stdio.h>

山寨：百度

**举例**
Argc：argument count（参数数量）
Argv：argument variant（参数内容）

```c
#include <stdio.h>
#define FILE_PATH "./file.c"
#define FILE_WAY_OW  "w"
       
int main(int argc , char** argv)
{
FILE* fp=NULL;
fp=fopen(FILE_PATH,FILE_WAY_OW);
if(NULL == fp)
	{
		printf("open failed. \r\n");
		return 0;
	}
return 0;
}
       
root@hejin-desktop:/home/helloworld# gedit ./main.c
root@hejin-desktop:/home/helloworld# gcc -o ./hj ./main.c
root@hejin-desktop:/home/helloworld# ./hj   //  ./打开这个文件并且执行
root@hejin-desktop:/home/helloworld# ls
file.c  hj  main.c
```


​       

改进后
```c
#include <stdio.h>
#define FILE_PATH "./file.c"
#define FILE_WAY_OW  "w"
#define MAIN_ARG_AMOUNT 2
       
int main(int argc , char** argv)
{
FILE* fp=NULL;
       
if(MAIN_ARG_AMOUNT != argc)
	{
		printf("usage:%s {FILE_PATH}.\r\n",argv[0]);	//usage:相当于便签
		return 0;
	}
       
fp=fopen(argv[1],FILE_WAY_OW);  //argv[1]:第一个参数。前面还有第零个参数
if(NULL == fp)
	{
		/*
		printf("%s\r\n",argv[0]);不可以，因为分配的空间不够
		printf("%c\r\n",argv[0]);不可以，因为分配的空间不够
		
		char d[10]="hello";
		printf("%s\r\n",d);打印的是这个字符串 hello
        printf("%s\r\n",&d[2]);打印的是llo，第二个字母之后的，从0开始
        printf("%x\r\n",d);打印的是d的首地址
        printf("%x\r\n",d[0]);打印的是h转换为ascii后的十六进制数
        printf("%x\r\n",&d[0]);打印的是h的地址
        
		printf("%x\r\n",argv[0]);可以打印出地址，%p也可以.%x:16进制
		printf("%p\r\n",argv[0]);
		*/
		printf("open failed. \r\n");
		return 0;
	}
return 0;
}
```

​       

```c
root@hejin-desktop:/home/helloworld# gcc -o ./hj ./main.c
root@hejin-desktop:/home/helloworld# ls
file.c  hj  main.c
root@hejin-desktop:/home/helloworld# ./hj ./test.c   
//./hj是第0个参数   ./test.c是第1个参数
root@hejin-desktop:/home/helloworld# ls
file.c  hj  main.c  test.c
root@hejin-desktop:/home/helloworld# 
       
```

char **：第一个星号是指向字符串的入口，第二个星号是确定哪一个

## 2019.3.20

**fwrite**

size_t fwrite(const void* buffer, size_t size, size_t count, FILE* stream);

返回值:返回实际写入的[数据块](https://baike.so.com/doc/2254878-2385689.html)数目

(1)buffer:是一个[指针](https://baike.so.com/doc/1043844-1104112.html)，void* ：万能指针  对fwrite来说，是要获取数据的地址;

(2)size:要写入内容的单字节数;    每次写几个

(3)count:要进行写入size字节的[数据项](https://baike.so.com/doc/6734126-6948491.html)的个数;   写几次

(4)stream:目标[文件指针](https://baike.so.com/doc/6178200-6391443.html);

(5)返回值：实际写入的数据项个数count。也就是写的总个数

size_t:   typedef unsigned long int size_t


```c
#include <stdio.h>
#include <stdlib.h>
#define FILE_PATH "./file.c"
#define FILE_WAY_OW  "w+"
#define MAIN_ARG_AMOUNT 2

int main(int argc , char** argv)
{
FILE* fp=NULL;

if(MAIN_ARG_AMOUNT != argc)
{
printf("usage:%s {FILE_PATH}.\r\n",argv[0]);
return 0;
}

/*********************************/
char ch[15];
scanf("%s",ch);
fp=fopen(argv[1],FILE_WAY_OW);
if(fwrite(ch,sizeof(ch),1,fp)!=1)
{
printf("open failed. \r\n");
return 0;
}
return 0;
}
/*******************************/


//或者
/*********************************/
//size_t fwrite(const void* buffer, size_t size, size_t count, FILE* stream);
char* s="hello wprld\r\n";
fp=fopen(argv[1],FILE_WAY_OW);

size_t w_count=0;	//初始化
w_count=fwrite("hello world!",1,11,fp);
printf("w_count:%ld\r\n",w_count);	//值为12.就是写入的总个数

int a=97;
w_count=fwrite(&a,1,sizeof（1）,fp);// 文件中显示了a

if(fwrite(s,sizeof(ch),1,fp)!=1)
{
printf("open failed. \r\n");
return 0;
}
return 0;
}
/*******************************/
```



**fread**

fread是一个函数，它从文件流中读数据，最多读取count个项，每个项size个字节，如果调用成功返回实际读取到的项个数(小于或等于count)，如果不成功或读到文件末尾返回 0。

**size_t** **fread (** **void** ******buffer***,** **size_t** *size***,** **size_t** *count***,** **FILE** ******stream***) ;**

*buffer*：用于接收数据的[内存地址](https://baike.so.com/doc/6467825-6681520.html)，也就是要存储到哪的数据地址

*size*：**要读的每个数据项的字节数**，单位是[字节](https://baike.so.com/doc/1114609-1179328.html)

*count*：**要读**count个数据项，每个数据项size个字节.

stream：输入流

[折叠](https://baike.so.com/doc/1708135-1805931.html#)**返回值**

返回真实读取的项数，若大于count则意味着产生了错误。另外，产生错误后，文件位置指示器是无法确定的。若其他stream或buffer为空指针，或在unicode模式中写入的字节数为奇数，此函数设置errno为EINVAL以及返回0.

**fseek**

int fseek(FILE *stream, long offset, int fromwhere);函数设置文件指针stream的位置。

如果执行成功，stream将指向以fromwhere为基准，偏移offset([指针](https://baike.so.com/doc/1043844-1104112.html)[偏移量](https://baike.so.com/doc/6117681-6330824.html))个字节的位置，函数返回0。如果执行失败(比如offset取值大于等于2*1024*1024*1024，即long的正数范围2G)，则不改变stream指向的位置，函数返回一个非0值。

fseek函数和lseek函数类似，但lseek返回的是一个off_t数值，而fseek返回的是一个整型。

int fseek( FILE *stream, long offset, int origin );

第一个参数stream为文件[指针](https://baike.so.com/doc/1043844-1104112.html)，文件流

第二个参数offset为[偏移量](https://baike.so.com/doc/6117681-6330824.html)，正数表示正向偏移，负数表示负向偏移

第三个参数origin设定从文件的哪里开始偏移,可能取值为:**SEEK_CUR、 SEEK_END 或 SEEK_SET**

SEEK_SET: 文件开头：是个宏，真实值是0	#define SEEK_SET 0

SEEK_CUR: 当前位置: 是个宏，真实值是1

SEEK_END: 文件结尾: 是个宏，真实值是2

其中SEEK_SET,SEEK_CUR和SEEK_END依次为0，1和2.

简言之:

fseek(fp,100L,0);把stream指针移动到离文件开头100字节处;

fseek(fp,100L,1);把stream指针移动到离文件当前位置100字节处;

fseek(fp,-100L,2);把stream指针退回到离文件结尾100字节处。

使用实例:

```c
#include <stdio.h>
#include <stdlib.h>
#define FILE_PATH "./file.c"
#define FILE_WAY_OW  "w+"
#define MAIN_ARG_AMOUNT 2

int main(int argc , char** argv)
{
FILE* fp=NULL;
size_t w_count=0,r_count=0;
int a=97,b=0,s_count=0;	

if(MAIN_ARG_AMOUNT != argc)
	{
		printf("usage:%s {FILE_PATH}.\r\n",argv[0]);
		return 0;
	}
//char ch[15];
//scanf("%s",ch);
	fp=fopen(argv[1],FILE_WAY_OW);
	if(NULL == fp)
		{
			printf("open failed. \r\n");
			return 0;
		}

	w_count=fwrite(&a,1,sizeof(1),fp);
	printf("w_count:%ld\r\n",w_count);
	if(w_count <= 0)
		{
			printf("fwrite failed. \r\n");
			return 0; 
		}
	else
		{
			s_count=fseek(fp,-sizeof(a),SEEK_CUR);
			printf("s_count:%d\r\n",s_count);
			if(s_count==0)
				{
					r_count=fread(&b,sizeof(int),sizeof(a)/sizeof(int),fp);
					printf("r_count:%ld\r\n",r_count);
					if(r_count <= 0)
						{	
							printf("fread failed. \r\n");
							return 0; 
						}
					else
						{
							printf("b:%d\r\n",b);
						}

				}
           else
				{
					printf("fseek failed. \r\n");
					return 0;
				}
		}
	fclose(fp);
return 0;
}

```

封装

```c
#include <stdio.h>
#include <string.h>
#define FILE_PATH "./file.c"
#define FILE_WAY_OW  "a+"
#define MAIN_ARG_AMOUNT 3
#define WRITE_TIME 1
#define READ_TIME 1

typedef unsigned char u8_t;
typedef unsigned int u32_t;
typedef char i8_t;

typedef struct
{
	FILE* fp;
	u8_t path_name[64];
	u8_t write_buf[1024];  //代表文件里写的内容
	u8_t read_buf[1024];//要存储的位置 	
}MYFILE_T;

i8_t file_open(MYFILE_T *myfile)
{
	myfile->fp=fopen(myfile->path_name,FILE_WAY_OW);
	if(NULL == myfile->fp)
		{
			printf("open failed. \r\n");
			return -1;
		}
	return 0;	
}

i8_t file_write(MYFILE_T *myfile)
{
	size_t w_count=0;

	memset(myfile->write_buf,0,sizeof(myfile->write_buf));	
	w_count=fwrite(myfile->write_buf,sizeof(myfile->write_buf),WRITE_TIME,myfile->fp);
	printf("w_count:%ld\r\n",w_count);
	if(w_count <= 0)
		{
			printf("fwrite failed. \r\n");
			return -1; 
		}
	printf("fwrite %ld bytes success.\r\n",w_count);	
}

i8_t file_read(MYFILE_T *myfile)
{
	int s_count=0;
	size_t r_count=0;
	s_count=fseek(myfile->fp,-sizeof(myfile->write_buf)*WRITE_TIME,SEEK_CUR);
	printf("s_count:%d\r\n",s_count);
	if(s_count==0)
	{
		r_count=fread(myfile->read_buf,sizeof(myfile->write_buf)*WRITE_TIME,sizeof(myfile->write_buf)*WRITE_TIME/
				READ_TIME,myfile->fp);
		printf("r_count:%ld\r\n",r_count);
		if(r_count <= 0)
		{	
			printf("fread failed. \r\n");
			return -1; 
		}
		else
		{
			printf("fread:%ld bytes\r\n",r_count);
			printf("fread content:%s\r\n",myfile->read_buf);
		}

		}
		else
		{
			printf("fseek failed. \r\n");
			return -1;
		}

}

void file_close(MYFILE_T *myfile)
{
	fclose(myfile->fp);
};

int main(int argc , char** argv )
{
	MYFILE_T myfile;

	if(MAIN_ARG_AMOUNT != argc)
	{
		printf("usage:%s {FILE_PATH}  {write_string}.\r\n",argv[0]);
		return 0;
	}

	memset(myfile.fp,0,sizeof(myfile));
	memcpy(myfile.path_name,argv[1],strlen(argv[1]));
	memcpy(myfile.write_buf,argv[2],strlen(argv[2]));

	if(-1==file_open(&myfile))   return 0;
	if(-1==file_write(&myfile))  return 0;
	if(-1==file_read(&myfile))  return 0;
	file_close(&myfile);
	
	return 0;
}

```


# 错误

1. a->next   这里的'->'   只能用在指针上    而你定义a为[结构体](http://www.so.com/s?q=%E7%BB%93%E6%9E%84%E4%BD%93&ie=utf-8&src=internal_wenda_recommend_textn)[实例](http://www.so.com/s?q=%E5%AE%9E%E4%BE%8B&ie=utf-8&src=internal_wenda_recommend_textn)而不是  指针    
   你得这样写：p=a.next;
# 文件操作总结

linux基础+linux应用编程

linux基础：装环境，一些基本命令

linux应用编程：文件编程

一. 文件编程

（1）基本流程：打开文件→文件读/写→关闭文件

**打开文件**：路径+文件名；打开方式（形态流）→得到一个文件指针（文件流/句柄）FILE*，唯一的，一个文件仅		   此一个

* FILE* fopen(char* pathname,char* mode)  →char*：字符串，mode（方式）

**文件读/写**：确定是哪个文件（通过FILE*确定）；读/写的缓冲区（buf）及方式(每次读多少，分几次)→得到读/写			的个数。指针都是8个字节（64位电脑）

* size_t fwrite（void* buf，size_t  *size*，size_t  *size，FILE* fp）；

  * **typedef long int size_t**

  * 返回的是读/写成功的个数

  * 除了数组，函数剩下的都要加&取地址

* size_t fread（void* buf，size_t  *size*，size_t  *size，FILE* fp）；

**关闭文件**：确定哪个文件（通过FILE*确定）

* fclose（FILE* fp）；

（2）具体操作：

**打开：fopen**

* 入参：路径+文件名；方式
  - r  w  a:和方式相关
  - b    t ：和形式先关
  -  +：和权限相关

**文件打开后一定要关闭**

**fread和fwrite的设计思路相同**

**fopen和open的区别**

**文件操作的本质，并不是直接对磁盘进行操作，而是建立一块缓存。待操作结束，才会将文件写入内存。**

**文件的两个指针，一个是FILE * 文件流，另一个是文件内部指针，他会随着他写入的字节，不断的往后去移动**

**文件内部指针：fseek（FILE * fp，long offset, int fromwhere）；

```c
#include <stdio.h>
#include <string.h>

int main(int argc,char** argv)
{
	FILE* fp1=NULL , *fp2=NULL;
	char write_buf[10]="hello";
	char read_buf[10]={0};//dan yin hao shi zifu,shuang yinhao shi zifuchuan
	char read2_buf[10]={0}; 
	size_t w_count=0,r_count=0,w2_count=0,r2_count=0;
	int s_count=0,s2_count=0;
		


	fp1=fopen("./name1","w+");
	if(fp1==NULL)
	{
		printf("f1open failed.\r\n");
		return 0;	
	}
	w_count=fwrite(write_buf,strlen(write_buf),1,fp1);
	if(w_count==0)
	{
		printf("fwrite failed.\r\n");
		return 0;
	}
	s_count=fseek(fp1,0,SEEK_SET);
	if(s_count!=0)
	{
		printf("fseek failed.\r\n");
		return 0;
	}		
	r_count=fread(read_buf,strlen(write_buf),1,fp1);
	if(r_count==0)
	{
		printf("fread failed.\r\n");
		return 0;
	}
	printf("read_buf.%s\r\n",read_buf);
	

	fp2=fopen("./name2","wt+");
	if(fp2==NULL)
	{
		printf("f2open failed.\r\n");
		return 0;	
	}
	w2_count=fwrite(read_buf,strlen(write_buf),1,fp2);
	if(w2_count==0)
	{
		printf("fwrite failed.\r\n");
		return 0;
	}
	s2_count=fseek(fp2,0,SEEK_SET);
	if(s2_count!=0)
	{
		printf("fseek failed.\r\n");
		return 0;
	}
	r2_count=fread(read2_buf,strlen(write_buf),1,fp2);
	if(r2_count==0)
	{
		printf("fread failed.\r\n");
		return 0;
	}
	printf("read2_buf:%s\r\n",read2_buf);
	fclose(fp1);
	fclose(fp2);
}
//将name1的文件保存到name2中
```

改进后

```c
#include <stdio.h>
#include <string.h>

int main(int argc,char** argv)
{
	FILE *fp1=NULL , *fp2=NULL;
	char write_buf[10]="hello";
	char read_buf[10]={0};//dan yin hao shi zifu,shuang yinhao shi zifuchuan
	char read2_buf[10]={0}; 
	size_t w_count=0,r_count=0;
	int s_count=0,s2_count=0;
		


	fp1=fopen("./name1","w+");
	fp2=fopen("./name2","w+");
	if(fp1==NULL || fp2==NULL)
	{
		printf("fopen failed.\r\n");
		return 0;	
	}
	w_count=fwrite(write_buf,strlen(write_buf),1,fp1);
	if(w_count==0)
	{
		printf("fwrite failed.\r\n");
		return 0;
	}
	s_count=fseek(fp1,0,SEEK_SET);
	if(s_count!=0)
	{
		printf("fseek failed.\r\n");
		return 0;
	}		
	r_count=fread(read_buf,strlen(write_buf),1,fp1);
	if(r_count==0)
	{
		printf("fread failed.\r\n");
		return 0;
	}
	printf("read_buf.%s\r\n",read_buf);
//fread之后，在文件中的光标继续呆在fseek设置的那个位置，依情况而定
	w_count=fwrite(read_buf,strlen(read_buf),1,fp2);
	if(w_count==0)
	{
		printf("fwrite failed.\r\n");
		return 0;
	}	
    //只写了没有读所以不用seek
	fclose(fp1);
	fclose(fp2);
}
```

