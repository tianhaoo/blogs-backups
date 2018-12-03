---
title: cppSTL
date: 2018-01-20 00:34:11
categories:
- 算法
tags:
  - cpp
  - datastructure
---

STL即标准模板库（Standard Template Library)，

STL是一些“容器”的集合，这些“容器”有list,vector,set,map等，STL也是算法和其他一些组件的集合。

这里的“容器”和算法的集合指的是世界上很多聪明人很多年的杰作。

下面整理一些常见的容器的用法



## stack(栈)

*stack（堆栈） 是一个容器类的改编，为程序员提供了堆栈的全部功能，——也就是说实现了一个先进后出（FILO）的数据结构。*

<!--more-->
* empty() 堆栈为空则返回真
* pop() 移除栈顶元素
* push() 在栈顶增加元素
* size() 返回栈中元素数目
* top() 返回栈顶元素


## queue(队列)

*C++队列是一种容器适配器，它给予程序员一种先进先出(FIFO)的数据结构。*

* back() 返回一个引用，指向最后一个元素
* empty() 如果队列空则返回真
* front() 返回第一个元素
* pop() 删除第一个元素
* push() 在末尾加入一个元素
* size() 返回队列中元素的个数





## vector(不定长数组)
*vector(向量): C++中的一种数据结构,确切的说是一个类.它相当于一个动态的数组,当程序员无法知道自己需要的数组的规模多大时,用其来解决问题可以达到最大节约空间的目的.*


* vector <int> M(a,b);   //在M里装a个b    
* vector <int> N(a);     //在N里装a个0
* v[i]或v.at(i)    //返回v[i]的值
* v.size();        //返回v数组元素总个数
* v.front();       //返回v数组第一个元素的值
* v.back();        //返回v数组最后一个元素的值
* v.clear();       //清空v数组
* v.begin();       //返回v数组第一个数的下标
* v.end();         //返回v数组最后一个数的下标
* v.empty();       //判断v数组是否为空，是空则返回1(true)，非空（有元素）则返回0(false)
* v.swap(v1);      //v1是另一个动态数组，将v和v1元素互换
* swap(v,v1);      //同上

## list(双向链表)

*STL中的list就是一双向链表，可高效地进行插入删除元素。List 是C++标准程式库中的一个类，可以简单视之为双向连结串行，以线性列的方式管理物件集合。list 的特色是在集合的任何位置增加或删除元素都很快，但是不支持随机存取。*

<!--more-->

> list 内部以数据结构的双向连结串行实做，内部元素并非放置于连续大块内存中，而是散落于内存各处，互相以link串接起来，每个元素都只知道其前一个元素以及下一个元素的位置。故要走访整个list，必须从第一个元素开始逐个往下寻访，不支持随机存取(Random Access)。 list 的强项是高效的插入以及删除，于list插入或删除时只需要改动元素的link字段，不需要搬动元素，代价相对便宜。
list 在经常需要于集合内部任意位置(即除了头尾以外的其他位置) 频繁增删元素的工作上表现优秀。若仅需要于集合尾端增删元素，那应该优先考虑vector容器，若仅于头尾二端增删元素，那应该优先考虑deque容器。

#### 构造

* list<int> c0; //空链表
* list<int> c1(3); //建一个含三个默认值是0的元素的链表
* list<int> c2(5,2); //建一个含五个元素的链表，值都是2
* list<int> c4(c2); //建一个c2的copy链表
* list<int> c5(c1.begin(),c1.end()); ////c5含c1一个区域的元素[First, Last)。

#### 方法

* assign() //分配值，有两个重载：
* c1.assign(c2.begin(), c2.end())
* c1.assign(7,4) //c1中现在为7个4,c1(4,4,4,4,4,4,4)。
* back() //返回最后一元素的引用：
* begin() //返回第一个元素的指针(iterator)
* clear() //删除所有元素
* empty() //判断是否链表为空
* end() //返回最后一个元素的下一位置的指针(list为空时end()=begin())
* erase() //删除一个元素或一个区域的元素(两个重载)
* front() //返回第一个元素的引用：
* insert() //在指定位置插入一个或多个元素(三个重载)：
* max_size() //返回链表最大可能长度(size_type就是int型)：
* merge() //合并两个链表并使之默认升序(也可改)：
* pop_back() //删除链表尾的一个元素
* pop_front() //删除链表头的一元素
* push_back() //增加一元素到链表尾
* push_front() //增加一元素到链表头
* rbegin() //返回链表最后一元素的后向指针(reverse_iterator or const)
* rend() //返回链表第一元素的下一位置的后向指针
* remove //()删除链表中匹配值的元素(匹配元素全部删除)
* remove_if() //删除条件满足的元素(会遍历一遍链表)
* resize() //重新定义链表长度(两重载)：
* reverse() //反转链表:
* size() //返回链表中元素个数
* sort() //对链表排序，默认升序(可自定义)
* splice() //对两个链表进行结合(三个重载)
* swap() //交换两个链表(两个重载)
* unique() //删除相邻重复元素(断言已经排序，因为它不会删除不相邻的相同元素)

#### 使用list的成员函数push_back和push_front插入一个元素到list中

* cList. push_back(‘a’); //把一个对象放到一个list的后面
* cList. push_front (‘b’); //把一个对象放到一个list的前面
* 使用list的成员函数empty()判断list是否为空


#### 用list< char >::iterator得到指向list的指针

```
list< char>::iterator charIterator;  
for(cIterator = cList.Begin();cIterator != cList.end();cIterator++)  
{  
    printf(“%c”, *cIterator);  
} //输出list中的所有对象  
```

*说明：cList.Begin()和cList.end()函数返回指向list< char >::iterator的指针，由于list采用链表结构，因此它不支持随机存取，因此不能用cList.begin()+3来指向list中的第 四个对象，vector和deque支持随机存取。*
#### 用STL的通用算法count()来统计list中的元素个数

```
int cNum;
char ch = ’b’;
cNum = count(cList.Begin(), cList.end(), ch); //统计list中的字符b的个数
```

*说明：在使用count()函数之前必须加入`#include <algorithm>`*

#### 用STL的通用算法count_if ()来统计list中的元素个数

```
const char c(‘c’);  
class IsC  
{  
public:  
    bool operator() ( char& ch )  
    {  
        return ch== c;  
    }  
};  

int numC;  
numC = count_if (cList.begin(), cList.end(),IsC());//统计c的数量；  
```

#### 使用STL通用算法find()在list中查找对象

```
list<char >::iterator FindIterator;  
FindIterator = find(cList.begin(), cList.end(), ‘c’);  
If (FindIterator == cList.end())  
{  
    printf(“not find the char ‘c’!”);  
}  
else  
{  
    printf(“%c”, * FindIterator);  
}  
```

#### 使用STL通用算法find_if()在list中查找对象

```
const char c(‘c’);  
class c  
{  
public:  
    bool operator() ( char& ch )  
    {  
        return ch== c;  
    }  
};  

list<char>::iterator FindIterator  
FindIterator = find_if (cList.begin(), cList.end(),IsC());//查找字符串c；  
```

#### 使用list的成员函数insert插入一个对象到list中
```
cList.insert(cLiset.end, ‘c’); ///在list末尾插入字符‘c’  

char ch[3] ={‘a’, ‘b’, ‘c’};  
cList.insert(cList.end, &ch[0], & ch[3] ); //插入三个字符到list中  
说明：insert()函数把一个或多个元素插入到指出的iterator位置。元素将出现在 iterator指出的位置以前。  
如何在list中删除元素  
cList.pop_front(); //删除第一个元素  
cList.pop_back(); //删除最后一个元素  
cList. Erase(cList.begin()); //使用iterator删除第一个元素；  
cList. Erase(cList.begin(), cList.End()); //使用iterator删除所有元素；  
cList.remove(‘c’); //使用remove函数删除指定的对象；  

list<char>::iterator newEnd;  
//删除所有的’c’ ,并返回指向新的list的结尾的iterator  
newEnd = cList.remove(cList.begin(), cList.end(), ‘c’);   
```

## map

待填..
