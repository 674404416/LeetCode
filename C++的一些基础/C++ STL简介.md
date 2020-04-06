# C++ STL简介

本文章参考整理自https://blog.csdn.net/S_999999/article/details/86428947

### 1.简单介绍

C++ STL（标准模板库）是一套功能强大的 C++ 模板类，提供了通用的模板类和函数，这些模板类和函数可以实现多种流行和常用的算法和数据结构，如向量、链表、队列、栈等。

**STL的一个重要特点就是数据结构和算法的分离。例如，STL中sort()函数是完全通用的，你可以用它来操作几乎任何数据集合，包括链表，容器和数组。**

**STL另一个重要特性是它不是面向对象的，主要依赖于模版，而不是封装和继承**

C++标准模板库的核心包括以下三个组件：

| 容器（Containers）  | 容器是用来管理某一类对象的集合。C++ 提供了各种不同类型的容器，比如 deque、list、vector、map 等。 |
| ------------------- | ------------------------------------------------------------ |
| 算法（Algorithms）  | 算法作用于容器。它们提供了执行各种操作的方式，包括对容器内容执行初始化、排序、搜索和转换等操作。 |
| 迭代器（iterators） | 迭代器用于遍历对象集合的元素。这些集合可能是容器，也可能是容器的子集。 |

### 2.容器

#### 1.vector

​	向量，可变大小数组。支持**随机访问**。适用于读写，删插比较慢（同数组）。



​	PS：什么是随机访问？

​		**随机访问**就是可以直接访问目标地址，如在数组中访问第四个元素就是a[3]。

​		而链表是不能随机访问的，如果想在链表中访问第四个元素就要从第一个元素循环到第四个元素，即**顺序访问**。

```c++
#include<vector>
//声明和初始化
vector<int> vec;        //声明一个int型向量
vector<int> vec(5);     //声明一个初始大小为5的int向量
vector<int> vec(10, 1); //声明一个初始大小为10且值都是1的向量
vector<int> vec(tmp);   //声明并用tmp向量初始化vec向量
//容量
vec.size();           //大小
vec.capacity();        //真实大小
vec.max_size();        //最大容量
vec.resize();          //更改大小
vec.empty();           //判空
//修改
vec.push_back();      //末尾添加元素
vec.insert();           //任意位置插入元素
 
vec.pop_back();      //末尾删除元素
vec.erase();           //任意位置删除元素 
 
vec.swap();           //交换两个元素
vec.clear();           //清空元素
//元素访问
vec[1];       //下标访问,并不会检查是否越界
vec.at(1);     //at方法访问,以上两者的区别就是at会检查是否越界
 
vec.front();     //访问第一个元素
vec.back();      //访问最后一个元素 
 
int* p = vec.data();    //返回一个指针,可行的原因在于vector在内存中就是一个连续存储的数组，所以可以返回一个指针指向这个数组。这是是C++11的特性。
 
```

配套算法

```c++
//for each循环遍历
for (int i : vec)
{
    cout << i << " ";
}

//普通遍历
for (int i = 0; i < vec.size(); i++)
{
    cout << vec.at(i) << " ";
}

//排序
sort(vec.begin(), vec.end()); //采用的是从小到大的排序
//如果想从大到小排序，可以采用下面的反转函数，也可以采用下面方法:
bool Comp(const int& a, const int& b) {
    return a > b;
}
sort(vec.begin(), vec.end(), Comp);

//翻转
#include <algorithm>
reverse(vec.begin(), vec.end());
```



#### 2.list

双向链表，支持**双向顺序访问**，插入删除快。

适用于大量插入删除

```c++
#include<list>
	//初始化
	list<int> lis;
	list<int> lis1(10, 1);
	list<int> lis2(10);
	list<int> lis3(lis);
	if (lis.empty())//判断是否为空
		cout << "是空的！" << endl;
	

	//修改
	lis.push_back(1);                        // 尾后压入元素
	lis.push_front(2);                       // 队头压入元素
	lis.push_back(111);
	lis.push_back(222);

	lis.pop_back();                         // 尾后弹出一个元素
	lis.pop_front();                        // 队头弹出一个元素

	lis.insert(lis.begin(), 1);              // 某个位置插入元素(性能好)

	lis.remove(2);                          // 删除某个元素(和所给值相同的都删除)
	lis.erase(--lis.end());                   // 删除某个位置的元素(性能好)


	 //元素的访问
	lis.front();
	lis.back();
```

算法

c++11新出的for each循环很方便，省去了定义迭代器

```
	//迭代
	for (int i : lis)
	{
		cout << i << " ";
	}
	cout << endl;

	list<int>::iterator it;

	for (it = lis.begin(); it != lis.end(); it++)
		cout << *it << " ";
		
	//排序
	lis.sort();
	//如果想改变排序顺序，可以定义一个比较大小的函数，写入括号内，函数写法同vector
	
	//倒置
	lis.reverse();                          

```



#### 3.forward_list

单向链表，在存储方面list会消耗更多的空间，插入和删除元素会稍微消耗更多的时间。Forward_list的迭代器是虽然是单向的，但是它比list效率高。



#### 4.deque

双端队列，支持快速随机访问。折中了vector和list。阅读下面的操作你会发现和上面的几乎无差别，

```c++
#include<queue>

//初始化
	deque<int> d1;
	deque<int> d2(d1);
	deque<int> d3(10, 1);

//判空
	dl.empty()     
        
	//元素插入访问取出
	d1.push_back(4);
	d1.push_front(4);
	d1.front();
	d1.back();
	d1.pop_back();
	d1.pop_front();
	//访问
	for (int i : d1)
	{
		cout << i << " ";
	}
	//排序
	sort(d1.begin(),d1.end());
	//翻转
	reverse(d1.begin(), d1.end());

```



#### 5.queue

普通队列，先进先出。最重要的是队列没有迭代器，所以不能进行遍历访问，排序和翻转。

PS:为什么queue（队列）和stack（栈）没有迭代器？

​	**因为迭代器对队列和栈没有意义！**对于队列来说，你永远只能访问队首的元素，就算有迭代器，你也不能顺序访问其他的元素。而对于栈来说，你永远只能访问队尾的元素。所以就没有迭代器了。

```c++
#include<queue>
//初始化
	queue<int> q1;
	//判空
	q1.empty();
	//插入删除 队列先进先出，所以插入是在队尾插入，出是在队首出
	q1.push(3);
	q1.pop();

	//头尾元素
	q1.front();
	q1.back();
	
```



#### 6.stack



```c++
#include<stack>
//初始化
	stack<int> s1;
	//判空
	s1.empty();
	//插入删除 队列先进先出，所以插入是在队尾插入，出是在队首出
	s1.push(3);
	s1.pop();

	//返回栈顶元素（不出）
	s1.top();


```



#### 7.string

```c++
#include<string>
string str1 = "Hello Ace";            // string的几种构造方法
	string str2("Hello World");
	string str3(str1, 6);                // 从str1下标6开始构造， str3 -> Ace
	string str4 = str2.substr(0, 5);    // 求子串： str4 -> Hello
	string str5 = str2.substr(6);        // 求子串： str5 -> World
	string str6 = str2.substr(6, 11);    // 求子串： str6 -> World

	string str8 = str2.replace(6, 5, "Game");    // 替换：str8 -> Hello Game 从位置6开始，删除5个字符，并替换成"Game"

	string str9 = str2.append(", Hello Beauty");// 追加字符串： str9 -> Hello World, Hello Beauty

	auto pos1 = str1.find("Ace");    // 查找字符串:pos1 -> 6 ,返回第一次出现字符串的位置，如果没找着，则返回npos

	int res = str1.compare("Hello, Ace");        // 比较字符串： res -> -1, 根据str1是等于、大于还是小于参数指定的字符串， 返回0、整数或者负数
	
	//遍历
	for (char i : str1)
	{
		cout << i << " ";
	}
	cout<< endl;
	//排序
	sort(str1.begin(), str1.end());
	//翻转
	reverse(str2.begin(),str2.end());
	cout << str2 << endl;

```



### 3.关联容器

关联容器支持高效的关键字查询和访问。标准库一共定义了8个关联容器，最主要的类型是map和set。8个容器中，每个容器：

- 是一个map或者是一个set。map保存关键字-值对；set只保存关键字。
- 要求关键字唯一或者不要求。
- 保持关键字有序或者不保证有序。



#### 1.set

set只保存键值，优点是查找快。本身是有序的，不能插入重复的键值

```c++
	#include<set>

	set<int> s;
	s.empty();//判空
	s.size();//大小
	s.max_size();//set可能包含的最大的元素个数
	//首尾
	s.begin();
	s.end();

	//插入
	s.insert(1);
	s.insert(2);
	s.insert(3);
	//删除
	s.erase(2);

	//查找
	set<int>::iterator it = s.find(3);
	if (it != s.end())
		cout << "找到了" << endl;

	//翻转
	reverse(s.begin(), s.end());

```



#### 2.map

保存两个值key-value。

```c++
#include<map>
	map<string, int>   my_Map;
	my_Map.size();           //返回元素数目 
	my_Map.empty();          //判断是否为空 
	my_Map.clear();          //清空所有元素
	//插入
	my_Map["123"] = 111;
	my_Map.insert(pair<string, int>("hello", 000));
	//查找
	if (my_Map.find("123")!=my_Map.end())
	{
		cout << "yes!" << endl;
	}
	//遍历
	for (auto a : my_Map)
	{
		cout << a.first << " " << a.second;
	}
	//删除(也可以用迭代器删除)
	my_Map.erase("123");

```



### 4.一些重点

- 迭代器最常用到的就是begin和end成员。**其中begin成员负责返回指向第一个元素**。end成员则负责返回指向容器的“尾元素的下一个位置。**要特别注意end成员不是指向尾元素，而是指向尾元素的下一个位置！**
- 表达式语句 ++it。 建议使用前置++而非后置++。 在迭代器中**，前置++的效率高于后置++**。
  - 如for(int i=0;i<100;++i)这句里++i和i++效果是一样的，但是++i效率高，涉及内存的因素
- 初始化语句：vector<int>::iterator it = b; 如果你的环境支撑C++11标准，那么强烈建议你写成auto it = b;，即使用类型自动推断关键字auto。使用auto使程序更为简洁，也不会出错，由编译器自动推断。
  