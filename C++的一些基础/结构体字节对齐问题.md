### 结构体字节对齐问题

​	在C/C++中 结构体是有字节对齐的

​	在本机系统中（不同系统不一样）
| 基本类型  | 占字节数 |
| :-------: | :------: |
|   char    |    1     |
|    int    |    4     |
|   float   |    4     |
|  double   |    8     |
|   long    |    4     |
| long long |    8     |
|   short   |    2     |

接下来看两个结构体

```c++
    struct A {
        int a;
        float b;
        long c;
        long long d;
    };
    struct B
    {
        char a;
        int b;
        float c;
        long d;
        long long e;
    };
```

输出sizeof(A)=24,sizeof(B)=24

明显是和只算类型字节数得出的答案是不同的，是因为在c++中有结构体字节对齐

#### 1.对齐规则

​	**结构体中一个变量占用n个字节，则该变量的起始地址必须能被n整除，不够就补空间**



如    INT+FLOAT+LONG=12    下一个long long 是占8个字节  12%8==4 不能被整除

所以要补4个空间 变成12+4+8=24

```shijiqingkc++
    struct A {
        int a;
        float b;
        long c;
        long long d;
    };
```



这是一个简单的结构体情况，其他的更为复杂 请看下面大佬的链接

>https://blog.csdn.net/daydreamingboy/article/details/8799734





