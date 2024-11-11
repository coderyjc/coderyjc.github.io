---
title: '笔记 | C++ STL'
date: 2020-02-15 13:09:24
tags:
---

# vector 向量

## 初始化 && 赋值

```c
//定义一个int类型的vector并向其中添加0, 1, 2, 3, 4
vector<int> v;
for (int i = 0; i < 5; i++) v.push_back(i);

//赋值
vector<int> v1 = v;

//拷贝构造
vector<int> v2(v);

//用v中的所有元素初始化v3
//迭代器类型
vector<int> v3(v.begin(), v.end());

//初始元素为 50
vector<int> v4(50);

//初始元素为 5个10
vector<int> v5(5, 10);

//用数组初始化容器
int arr[5] = { 0, 1, 2, 3, 4 };
vector<int> v6(arr, arr + sizeof(arr) / sizeof(int));

//用成员函数赋值
vector<int> v6;
v6.assign(v.begin(), v.end());

//交换
vector<int> v7;
v7.swap(v); //交换了v7和v的内容(实质是交换指针)


```

## 访问

```c
	vector<int> v;
	for (int i = 0; i < 5; i++) v.push_back(i);

	//作为起始端, 返回容器首部元素迭代器
	v.begin();
	
	//返回容器尾部元素迭代器
	v.end();

	//作为起始端, 返回容器尾部元素迭代器
	v.rbegin();

	//返回容器首部元素迭代器
	v.rend();

	//返回容器中第一个数据元素
	v.front();

	//返回容器中最后一个数据元素
	v.back();
```



## 个数/判空/重置/容量/预留

```c
	vector<int> v;
	for (int i = 0; i < 5; i++) v.push_back(i);
	
	//返回容器中元素的个数
	v.size();

	//判断容器是否为空
	v.empty();

	//重新指定容器的长度为20，
	//若容器变长，则以默认值 0 填充新位置。
	//如果容器变短，则末尾超出容器长度的元素被删除。
	v.resize(20);

	//重新指定容器的长度为20，
	//若容器变长，则以 1 填充新位置。
	//如果容器变短，则末尾超出容器长度的元素被删除。
	v.resize(20, 1);

	//返回容器的容量
	v.capacity();

	//容器预留50个元素长度，预留位置不初始化，元素不可访问
	v.reserve(50);
```

## 存取/插入/删除

```c
	vector<int> v;
	for (int i = 0; i < 5; i++) v.push_back(i);
	
	//返回索引 4 所指的数据，
	//如果越界，抛出out_of_range异常
	v.at(4);

	//越界时，运行直接报错
	v[4];

	//向迭代器 v.begin() + 3 所指向的位置插入 5 个 0
	v.insert(v.begin() + 1, 5, 0);

	//尾插 0
	v.push_back(0);

	//删除最后一个元素
	v.pop_back();

	//删除迭代器 v.begin() + 3 所指向的元素
	v.erase(v.begin() + 3);

	//删除迭代器 v.begin() + 3 和 v.begin() + 4 之间的元素
	v.erase(v.begin() + 3, v.begin() + 4);
	
	//清空容器中的元素
	v.clear();
```


# string 字符串

## 初始化

```c
	string s1; //无参构造
	string s2(5, 'x'); //5 个 x
	string s3("Hello World"); //Hello World
	string s4(s3); //拷贝构造
```

## 拼接

```c
	string s = "abc";
	string s2 = "def";
	string ch = "d";

	//下面两种方式等效
	//向末尾添加一个字符
	s += 'd';
	s += ch;

	//下面三种方式等效
	//向末尾链接字符串
	s += s2;
	s += "def";
	s.append(s2);

	//将 s2 在区间 [2, 最后] 的字符链接到 s 后面
	//下标从 0 开始
	s.append(s2, 2);

	//将 s2 的第 [1, 2] 个字符链接到 s 的结尾
	s.append(s2, 1, 2);
	cout << s << endl;

	// 将 5 个 x 链接到 s 的结尾
	s.append(5, 'x');

```


## 查找和替换

```c
	string s = "123456123456";

// 查找 ___ 返回找到的元素的下标

	// 查找 123 第一次出现的位置, 从前往后
	s.find("123"); 

	//从位置 4 开始, 查找 123 第一次出现的位置, 从前往后
	s.find("123", 4); 

	//从s中, 从第4个元素开始, 查找 123 中前3个字符[0, 3)第一次出现的位置
	s.find("123", 4, 3);

/*将 find 换为 rfind 为从后往前查找*/

//替换
	//将 s 中 [0, 4) 的元素替换为 "99"
	s.replace(0, 4, "99");
```

## 比较

```c
//区分大小写, 从头开始比较, 参考asc2码
//相等返回 0
//小于返回 -1
//大于返回 1

	string s1 = "abc";
	string s2 = "abd";

	s1.compare(s2);
```


## 子串/插入和删除

```c
	string s = "123456789";

//子串
	//返回 s 中 [1, 6] 的子串给 s_sub
	string s_sub = s.substr(1, 6);

//插入
	//在第一个位置前面插入 "aaa"
	s.insert(1, "aaa");

	//在第一个位置前面插入 3 个 a
	s.insert(1, 3, 'a');

//删除
	//删除 [0, 6) 上的元素
	s.erase(0, 6);

	//删除 下标 6 及之后的元素, 即[6, s.size())上的
	s.erase(6);
```

# deque 双端数组

## 初始化

```c
	deque<int> d;
	for (int i = 0; i < 5; i++) d.push_back(i);
	
	//赋值
	deque<int> d1 = d;

	//拷贝构造
	deque<int> d2(d);

	//用d中的所有元素初始化d3
	//迭代器类型
	deque<int> d3(d.begin(), d.end());

	//初始元素为 50
	deque<int> d4(50);

	//初始元素为 5个10
	deque<int> d5(5, 10);

	//用数组初始化容器
	int arr[5] = { 0, 1, 2, 3, 4 };
	deque<int> d6(arr, arr + sizeof(arr) / sizeof(int));

	//用成员函数赋值
	deque<int> d6;
	d6.assign(d.begin(), d.end());

	//交换
	deque<int> d7;
	d7.swap(d); //交换了v7和v的内容(实质是交换指针)
```



## 访问

```c
	deque<int> d;
	for (int i = 0; i < 5; i++) d.push_back(i);

	//作为起始端, 返回容器首部元素迭代器
	d.begin();

	//返回容器尾部元素迭代器
	d.end();

	//作为起始端, 返回容器尾部元素迭代器
	d.rbegin();

	//返回容器首部元素迭代器
	d.rend();

	//返回容器中第一个数据元素
	d.front();

	//返回容器中最后一个数据元素
	d.back();
```

## 个数/判空/重置

```c
	deque<int> d;
	for (int i = 0; i < 5; i++) d.push_back(i);
	
	//返回容器中元素的个数
	d.size();

	//判断容器是否为空
	d.empty();

	//重新指定容器的长度为20，
	//若容器变长，则以默认值 0 填充新位置。
	//如果容器变短，则末尾超出容器长度的元素被删除。
	d.resize(20);

	//重新指定容器的长度为20，
	//若容器变长，则以 1 填充新位置。
	//如果容器变短，则末尾超出容器长度的元素被删除。
	d.resize(20, 1);
```

## 存取/插入/删除

```c
	deque<int> d;
	for (int i = 0; i < 5; i++) d.push_back(i);
	
	//返回索引 4 所指的数据，
		//如果越界，抛出out_of_range异常
	d.at(4);

	//越界时，运行直接报错
	d[4];

	//向迭代器 d.begin() + 3 所指向的位置插入 5 个 0
	d.insert(d.begin() + 1, 5, 0);

	//头插 0
	d.push_front(0);

	//尾插 0
	d.push_back(0);

	//删除最后一个元素
	d.pop_back();

	//删除首元素
	d.pop_front();

	//删除迭代器 d.begin() + 3 所指向的元素
	d.erase(d.begin() + 3);

	//删除迭代器 d.begin() + 3 和 d.begin() + 4 之间的元素
	d.erase(d.begin() + 3, d.begin() + 4);

	//清空容器中的元素
	d.clear();

```

# list 链表

### 初始化

```c
	list<int> l1;

	//使用l1中元素的迭代器来初始化l2
	list<int> l2(l1.begin(), l1.end());
	
	//l3中的元素为 3 个 5
	list<int> l3(3, 5);
	
	//拷贝构造
	list<int> l4(l1);
```

### 数据的存取/删除/清空

```c
	list<int> lis;

	//返回第一个元素
	lis.front();

	//返回最后一个元素
	lis.back();

	//尾插元素 6
	lis.push_back(6);

	//头插元素 1
	lis.push_front(1);

	//删除尾部元素
	lis.pop_back();

	//删除头部元素
	lis.pop_front();

	//向迭代器指向的位置插入元素 5
	lis.insert(lis.begin(), 5);
	/*
		注意 : list 不支持随机访问, 所以如果想要向某一位置插元素
		只能用循环把迭代器 ++ 上去. 下同
	*/
	//向迭代器指向的位置插入5 个元素 5
	lis.insert(lis.begin(), 5, 5);

	//移除所有数据
	lis.clear();

	//删除begin到end区间内的数据, 返回下一数据的位置
	lis.erase(lis.begin(), lis.end());

	//删除pos位置的数据, 返回下一数据的位置
	lis.erase(lis.begin());

	//删除容器中所有的 10
	lis.remove(10);
```

### 容器中元素的个数/判空/容量重设/反转

```c
	//返回容器中元素的个数
	lis.size();

	//判断容器是否为空
	lis.empty();

	//重新指定容器的长度为20，
	//若容器变长，则以默认值填充新位置
	lis.resize(20);

	//重新指定容器的长度为20，
	//若容器变长，则以 1 填充新位置
	lis.resize(20, 1);

	//如果容器变短，则末尾超出容器长度的元素被删除

	//反转链表
	lis.reverse();
```

### 内置排序

```c
//默认升序
lis.sort();

//降序写仿函数
bool myCompare(int v1, int v2) {
	return v1 > v2;
}

lis.sort(myCompare);

//同理, 内置数据类型也要写仿函数
```
# queue 队列

#Cpp/STL/queue

**不能进行遍历, 不提供迭代器, 不支持随机访问**

```c
	queue<int> q;

	//拷贝构造函数
	queue<int> q1(q);

	//入队
	q.push(10);

	//出队
	q.pop();

	//返回队首元素
	q.front();
	
	//返回队尾元素
	q.back();

	//返回栈是否为空 空为1, 非空为0
	q.empty();

	//返回栈中元素的个数
	q.size();
```

# set/multiset 集合

#Cpp/STL/set #Cpp/STL/multiset

set和multiset的区别:set不允许相同的元素而multiset允许

不允许通过迭代器修改元素的值

但是可以删了再加进去

```c
//初始化
	set<int> s;

//拷贝构造
	set<int> s2(s);

//赋值
	s2 = s;

//交换
	s2.swap(s);

//元素的数目
	s.size();

//判空
	s.empty();

//插入
	s.insert(10);

//清除
	s.clear();

//删除
	//删除pos迭代器所指的元素，返回下一个元素的迭代器
	s.erase(pos);

	// 删除区间[beg，end）的所有元素，返回下一个元素的迭代器
	s.erase(beg, end); 

	//删除容器中值为elem的元素
	s.erase(elem);

//查找
	//查找键key是否存在，若存在，返回该键的元素的迭代器；
	//若不存在，返回 s.end()
	s.find(key);

	//返回第一个key>=keyElem元素的迭代器
	s.lower_bound(keyElem);
	
	//返回第一个key > keyElem元素的迭代器
	s.upper_bound(keyElem);

	//返回容器中key与keyElem相等的上下限的两个迭代器
	//返回的是 lower_bound 和 upper_bound 值
	pair<set<int>::iterator, set<int>::iterator> pa = s.equal_range(key);
```

### set更改默认排序

```c
class myCompare {
public:
	bool operator()(int v1, int v2) {
		return v1 > v2;
	}
};

void test01() {
	set<int, myCompare> s;

}

//自定义函数对象同理
```
# stack 栈

**栈不能遍历，不支持随机存取，只能通过top从栈顶获取和删除元素。**

```c
	stack<int> s;

	//拷贝构造函数
	stack<int> s1(s);

	//入栈, 向栈中压元素 10
	s.push(10);
	
	//出栈, 弹出栈顶元素
	s.pop();

	//返回栈顶元素值
	s.top();

	//返回栈是否为空 空为1, 非空为0
	s.empty();

	//返回栈中元素的个数
	s.size();
```
# map/multimap 映射

#Cpp/STL/map #Cpp/STL/multimap #Cpp/STL/pair

## 创建以string为键的map

map的直接插入不需要判断是否存在

不加这个判断函数也可以，有时候**加上了反而拉低了运行速度**。

```cpp
#include<string.h>

struct StrCmp {
	bool operator()(string l, string r) const { return strcasecmp(l.c_str(), r.c_str()) < 0; }
};
```

## map按照value排序

```cpp
struct CmpByValue {  
  bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {  
    return lhs.second > rhs.second;  
  }  
};

// ....
// 先转换为vector<pair>，再进行排序
vector<pair<int, int>> name_score_vec(name_score_map.begin(), name_score_map.end());
sort(name_score_vec.begin(), name_score_vec.end(), CmpByValue());
```



## pair对组

```c
//初始化
	pair<int, string> p = make_pair(1, "No.1");
	pair<int, string> p1(1, "No.1");
//访问

//返回第一个元素
	p1.first;

//返回第二个元素
	p1.second;

//赋值
	pair<int, string> p2 = p1;
```

## map操作

map不允许相同的键值的存在但是multimap允许

不允许通过迭代器修改相应的键值

但是可以删了重建

```c
//初始化
	map<int, int> m;

//拷贝构造
	map<int, int> m1(m);

//赋值
	m1 = m;

//交换
	m.swap(m1);

//元素数目
	m.size();

//判空
	m.empty();

//插入
	//插入之后是有返回值的, 返回值为一个迭代器和bool类型组成的对组
	//迭代器指向刚刚插入的位置
	//bool 返回是否插入成功

	//匿名对象
	m.insert(pair<int, int>(10, 10));
	
	//函数
	m.insert(make_pair(10, 10));
	
	//valueType
	m.insert(map<int, int>::value_type(10, 10));
	
	//类似python字典
	//这种方式的话, 如果key存在就会修改, 不存在就会创建一个
	m[10] = 10;


//访问
	//迭代器取出的是一个pair
	map<int, int>::iterator it = m.begin();
	(*it).first;
	(*it).second;

	map<int, int> s;
	//查找
	//查找键key是否存在，若存在，返回该键的元素的迭代器；
	//若不存在，返回 s.end()
	s.find(key);

	//返回第一个key>=keyElem元素的迭代器
	s.lower_bound(keyElem);

	//返回第一个key > keyElem元素的迭代器
	s.upper_bound(keyElem);

	//返回容器中key与keyElem相等的上下限的两个迭代器
	//返回的是 lower_bound 和 upper_bound 值
	pair<map<int, int>::iterator, map<int, int>::iterator> res = s.equal_range(key);

```

# priority queue 优先级队列

#Cpp/STL/priority_queue 

```cpp
#include<queue>
```

## 操作

```cpp
pq.empty() //如果队列为空返回真

pq.pop() //删除对顶元素

pq.push() //加入一个元素

pq.size() //返回优先队列中拥有的元素个数

pq.top() //返回优先队列对顶元素
```

## 设置优先级

```cpp
struct cmp1{  
    bool operator ()(int &a,int &b){  
        return a>b;//最小值优先  
    }
};

priority_queue<int,vector<int>,cmp1>pq;

// --------------------------------------------------

struct cmp2{  
    bool operator ()(int &a,int &b){  
        return a<b;//最大值优先  
    }  
};

priority_queue<int,vector<int>,cmp2>pq2;

// --------------------------------------------------

struct node  //自定义类型优先级设定
{
    friend bool operator< (node n1, node n2)//重载小于号，，不要重载大于号，可能报错
    {
        return n1.priority < n2.priority;// (priority)大的优先级高
    }
    int priority;
    int value;
};

priority_queue<node> pq4;
```

## 参考

- https://blog.csdn.net/chaiwenjun000/article/details/45457125
- 


# STL查找算法

### find

查找制定元素出现位置

```c
find(iterator begin, iterator end, value);
find(开始迭代器, 结束迭代器, 查找的元素)
//找到返回指定位置迭代器, 找不到返回.end()

//查找内置的数据类型

void test()
{
    vector<int>v;
    for (int i = 0; i < 10; i++)
        v.push_back(i);
    vector<int>v1;

    //查找容器中是否有 5  , 返回其第一次出现的位置
    vector<int>::iterator it = find(v.begin(), v.end(), 5);
}



//查找自定义数据类型(需要重载 == 号)
class Person
{
public:
    Person(string name, int age) : m_Age(age), m_Name(name) {}

    //重载==号, 让find知道如何对比 Person 数据类型
    bool operator==(const Person& p) {
        if (this->m_Age == p.m_Age && this->m_Name == p.m_Name)
            return true;
        else
            return false;
    }

    string m_Name;
    int m_Age;
};

void test()
{
    vector<Person> v;
    Person p1("aa", 10);
    Person p2("bb", 20);
    Person p3("cc", 30);
    Person p4("dd", 40);
    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);
    v.push_back(p4);

    //查找容器中是否有 p2 这个元素 , 返回其第一次出现的位置
    vector<Person>::iterator it = find(v.begin(), v.end(), p2);
    if (it == v.end())
        cout << "Not Found" << endl;
    else
        cout << "Name : " << (*it).m_Name << endl;
}

```

### find_if

按条件查找元素

```c
find_if(iterator beg, iterator end, _Pred);
find_if(开始迭代器, 结束迭代器, 函数或者谓词);

//查找内置数据类型

class GreatFive
{
public:
    bool operator()(int i)
    {
        return i > 5;
    }
};

void test()
{
    vector<int>v;
    for (int i = 0; i < 10; i++)
        v.push_back(i);
    //查找容器中是否有 5 这个元素 , 返回其第一次出现的位置, 找不到则返回 v.end()
    vector<int>::iterator it = find_if(v.begin(), v.end(), GreatFive());
    if (it == v.end())
        cout << "Not Found" << endl;
    else
        cout << *it << endl;
}



//查找自定义数据类型

class Person
{
public:
    Person(string name, int age)
    {
        this->m_Age = age;
        this->m_Name = name;
    }

    string m_Name;
    int m_Age;
};

class Greater20
{
public:
    int operator()(const Person& p)
    {
        return p.m_Age > 20;
    }
};

void test()
{
    vector<Person>v;
    Person p1("aa", 10);
    Person p2("bb", 20);
    Person p3("cc", 30);
    Person p4("dd", 40);
    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);
    v.push_back(p4);


//查找容器中是否有 满足条件的元素 , 返回其第一次出现的位置
    vector<Person>::iterator it = find_if(v.begin(), v.end(), Greater20());
    if (it == v.end())
        cout << "Not Found" << endl;
    else
        cout << "Name : " << (*it).m_Name << endl;
}
```

### adjacent_find

查找相邻重复元素

```c
adjacent_find(iterator beg, iterator end);
adjacent_find(开始迭代器, 结束迭代器);
//查找相邻重复元素。返回第一对相邻元素的第一个位置的迭代器, 未找到则返回.end()

void test()
{
    vector<int>v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(2);
    v.push_back(3);
    v.push_back(3);
    v.push_back(4);
    v.push_back(5);

    vector<int>::iterator it = adjacent_find(v.begin(), v.end());
    if (it == v.end())
        cout << "Not Found" << endl;
    else
        cout << *it << endl;
}
```



### binary_search

查找指定元素是否存在

```c
binary_search(iterator beg, iterator end, value);
binary_search(开始迭代器, 结束迭代器, 查找的元素);
//查找指定的元素，查到返回true 否则false
//只查找并表示有或没有, 不会返回有几个和它的位置
//在无序序列中不可用

void test()
{
	vector<int>v;
	v.push_back(1);
	v.push_back(2);
	v.push_back(3);
	bool it = binary_search(v.begin(), v.end(), 3);
	cout << it << endl;
}
```

### count

统计元素个数

```c
count(iterator beg, iterator end, value); 
count(开始迭代器, 结束迭代器, 统计的元素);

//统计内置数据类型

void test()
{
    vector<int>v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(2);
    v.push_back(3);
    v.push_back(3);
    int it = count(v.begin(), v.end(), 3);
    cout << it << endl;
}

//统计自定义数据类型

class Person
{
public:
    Person(string name, int age) : m_Age(age), m_Name(name) {}
 
//重载==号, 让 编译器 知道如何对比 Person 数据类型
    bool operator==(const Person& p)
    {
        if (this->m_Age == p.m_Age)
            return true;
        else
            return false;
    }

    string m_Name;
    int m_Age;
};

void test()
{
    vector<Person>v;
    Person p1("aa", 10);
    Person p2("bb", 20);
    Person p3("cc", 20);
    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);

    Person p("ee", 20);
    int cnt = count(v.begin(), v.end(), p);
    cout << cnt << endl;
}
```



### count_if

按条件统计元素个数

```c
count_if(iterator beg, iterator end, _Pred);
count_if(开始迭代器, 结束迭代器, 谓词);

//内置数据类型

class Great3
{
public:
    bool operator()(int i)
    {
        return i > 3;
    }
};

void test()
{
    vector<int>v;
    v.push_back(1);
    v.push_back(3);
    v.push_back(3);
    v.push_back(4);
    v.push_back(5);

    int it = count_if(v.begin(), v.end(), Great3());
    cout << it << endl;
}

//自定义数据类型

class Person
{
public:
    Person(string name, int age) : m_Age(age), m_Name(name) {}
    string m_Name;
    int m_Age;
};

class Greater20
{
public:
    int operator()(const Person& p)
    {
        return p.m_Age > 20;
    }
};

void test()
{
    vector<Person>v;
    Person p1("aa", 10);
    Person p2("bb", 20);
    Person p3("cc", 20);

    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);
    
    int cnt = count_if(v.begin(), v.end(), Greater20());
    cout << cnt << endl;
}
```

# STL-遍历算法


### for_each

用函数遍历元素

```c
for_each(iterator begin, iterator end, _func);
for_each(开始迭代器, 结束迭代器, _func函数或者函数对象);

//普通函数
void print(int i)
{
    cout << i << endl;
}

//仿函数
class Print
{
public:
    void operator()(int i)
    {
        cout << i << endl;
    }
};

void test()
{
    vector<int>v;
    for (int i = 0; i < 10; i++) v.push_back(i);

//普通函数对象
    for_each(v.begin(), v.end(), print); 

//重载了 () 的匿名函数对象
    for_each(v.begin(), v.end(), Print()); 
}
```



### transform

将一个容器中的元素搬运容器到另一个容器中, 前提是目标容器需要提前开辟空间

```c
transform(iterator begin1, iterator end1, iterator begin2, _func);
transform(原容器开始迭代器, 原容器结束迭代器, 目标容器开始迭代器,函数或者函数对象(回调函数));


class Transform
{
    // 在搬运的过程中可以进行一些操作
public:
    int operator()(int i)
    {
        return i;
    }
};

void test()
{
    vector<int> v;
    for (int i = 0; i < 10; i++)
        v.push_back(i);
    vector<int> v1;
    v1.resize(v.size()); //提前开辟空间
    transform(v.begin(), v.end(), v1.begin(), Transform()); //搬运
}
```

# STL排序算法

### sort

对容器内元素进行排序

```c
sort(iterator beg, iterator end, _Pred);
sort(开始迭代器, 结束迭代器, 谓词);
//按值查找元素, 找到返回指定位置迭代器, 找不到返回结束迭代器位置
//默认为升序排列

//内置数据类型
//降序排列
class Great3
{
public:
    bool operator()(int v1, int v2)
    {
        return v1 > v2;
    }
};

void test()
{
    vector<int>v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(3);
    v.push_back(4);
    v.push_back(5);

    sort(v.begin(), v.end(), Great3());

    for (auto i : v)
        cout << i << endl;
}


//自定义数据类型

class Person
{
public:
    Person(string name, int age) : m_Age(age), m_Name(name) {}

    string m_Name;
    int m_Age;
};

// 降序排列
class Greater20
{
public:
    bool operator()(const Person& p1, const Person& p2)
    {
        return p1.m_Age > p2.m_Age;
    }
};

void test()
{
    vector<Person>v;
    Person p1("aa", 10);
    Person p2("bb", 20);
    Person p3("cc", 20);
    Person p4("dd", 40);
    v.push_back(p1);
    v.push_back(p2);
    v.push_back(p3);
    v.push_back(p4);

    sort(v.begin(), v.end(), Greater20());

    for (auto i : v)
    {
        cout << i.m_Age
            << i.m_Name << endl;
    }

}


```



### random_shuffle

指定范围内的元素随机调整次序

```c
random_shuffle(iterator beg, iterator end);
random_shuffle(开始迭代器, 结束迭代器);

//伪随机

void test()
{
    vector<int>v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(3);
    v.push_back(4);
    v.push_back(5);

    random_shuffle(v.begin(), v.end());

    for (auto i : v)
        cout << i << endl;
}

//真随机(#include<time.h>)

void test()
{
    srand((unsigned int)time(NULL));  // 按系统时间进行随机
    vector<int>v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(3);
    v.push_back(4);
    v.push_back(5);
    random_shuffle(v.begin(), v.end());
    for (auto i : v)
        cout << i << endl;
}
```

### merge

两个容器元素合并, 并存储到另一容器中

```c
//注意：两个容器必须是有序的 , 而且在合并以后依然是有序的
//注意: 一定要在合并之后存放的容器中开辟空间
merge(iterator beg1, iterator end1, 
      iterator beg2, iterator end2, 
      iterator dest);
merge(容器1开始迭代器, 容器1结束迭代器, 
      容器2开始迭代器, 容器2结束迭代器, 
      目标容器开始迭代器);

void test()
{
    vector<int>v;
    vector<int>v1;
    vector<int>vt;
    for (int i = 0; i < 5; i++)
        v.push_back(i);
    for (int i = 5; i < 10; i++)
        v1.push_back(i);

    vt.resize(v.size() + v1.size());

    merge(v.begin(), v.end(), 
    		v1.begin(), v1.end(), vt.begin());

    for (auto i : vt)
        cout << i << " ";
}

```

### reverse

将容器内元素进行反转

```c
reverse(iterator beg, iterator end);
reverse(开始迭代器, 结束迭代器);

void test()
{
    vector<int>vt;
    for (int i = 0; i < 5; i++)
        vt.push_back(i);
    reverse(vt.begin(), vt.end());
    for (auto i : vt)
        cout << i << " ";
}
```

# STL集合算法

### set_intersection

> 使用时包含的头文件为#include < algorithem >

求两个容器的交集

```c
set_intersection(iterator beg1, iterator end1, 
                 iterator beg2, iterator end2, 
                 iterator dest);
set_intersection(容器1开始迭代器, 容器1结束迭代器, 
                 容器2开始迭代器, 容器2结束迭代器, 
                 目标容器开始迭代器);
//v3使用之前要开辟空间

void test() {
	vector<int> v1;
	vector<int> v2;
	vector<int> v3;
	for (int i = 0; i < 5; i++){
		v1.push_back(i);
		v2.push_back(i + 2);
	}

	v3.resize(v1.size() + v2.size());
	set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), v3.begin());

	for (auto i : v3) {
		cout << i << " ";
	}
	
}
```

### set_union



> 使用时包含的头文件为#include < algorithem >

求两个集合的并集

```c
set_union(iterator beg1, iterator end1, 
          iterator beg2, iterator end2, 
          iterator dest);

set_union(容器1开始迭代器, 容器1结束迭代器,
		  容器2开始迭代器, 容器2结束迭代器,
		  目标容器开始迭代器);

//两个集合必须是有序序列

//v3使用之前要开辟空间
void test() {
	vector<int> v1;
	vector<int> v2;
	vector<int> v3;
	for (int i = 0; i < 5; i++){
		v1.push_back(i);
		v2.push_back(i + 2);
	}

	v3.resize(v1.size() + v2.size());
	set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), v3.begin());

	for (auto i : v3) {
		cout << i << " ";
	}
	
}
```



### set_difference

> 使用时包含的头文件为#include < algorithem >

```c
set_difference(iterator beg1, iterator end1, 
      	iterator beg2, iterator end2, 
      	iterator dest);

set_difference(容器1开始迭代器, 容器1结束迭代器,
		  容器2开始迭代器, 容器2结束迭代器,
		  目标容器开始迭代器);

//v3使用之前要开辟空间
void test() {
	vector<int> v1;
	vector<int> v2;
	vector<int> v3;
	for (int i = 0; i < 5; i++){
		v1.push_back(i);
		v2.push_back(i + 2);
	}
	v3.resize(v1.size() + v2.size());
	set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), v3.begin());
	for (auto i : v3) {
		cout << i << " ";
	}
}
```


# STL拷贝和替换算法

### copy

容器内指定范围的元素拷贝到另一容器中

```c
copy(iterator eg, iterator end, iterator dest);
copy(开始迭代器, 结束迭代器, 目标起始迭代器);
//目标容器应该先开辟空间

void test()
{
    vector<int>v1;
    vector<int>v2;
    for (int i = 0; i < 5; i++)
        v1.push_back(i);
    v2.resize(v1.size());
    copy(v1.begin(), v1.end(), v2.begin());
    for (auto i : v2)
        cout << i << " ";
}
```

### replace

将容器内指定范围的所有旧元素修改为新元素

```c
replace(iterator beg, iterator end, 
				oldvalue, newvalue);
replace(开始迭代器, 结束迭代器, 旧元素, 新元素);

void test() {
	vector<int> v;
	v.push_back(1);	
	v.push_back(1);	
	v.push_back(2);
	v.push_back(3);
	replace(v.begin(), v.end(), 1, 10);
	for (auto i : v) {
		cout << i << " ";
	}
}

```



### replace_if

将区间内满足条件的元素, 替换成指定元素

```c
replace_if(iterator beg, iterator end, _pred, newvalue);
replace_if(开始迭代器,  结束迭代器, 谓词, 替换的新元素);

class Greater2 {
public:
	bool operator()(int x) {
		return x > 2;
	}
};

void test() {

	vector<int> v;
	v.push_back(1);
	v.push_back(1);
	v.push_back(2);
	v.push_back(3);
	replace_if(v.begin(), v.end(), Greater2(), 0);
	for (auto i : v) {
		cout << i;
	}
}

```

### swap

互换两个容器的元素

```c
swap(container c1, container c2);
swap(容器1, 容器2);

void test() {

	vector<int> v;
	v.push_back(1);
	v.push_back(1);
	v.push_back(2);
	v.push_back(3);
	vector<int> v1;
	swap(v, v1);
}
```


# STL填充算法


### accumulate

> 使用时包含的头文件为#include < numeric >

返回区间内容器元素累计总和

```c
accumulate(iterator beg, iterator end, value);
accumulate(开始迭代器, 结束迭代器, 起始值);

void test() {
	vector<int> v(5, 10);
	accumulate(v.begin(), v.end(), 0);
}
```

### fill 

> 使用时包含的头文件为#include < numeric >

向容器中填充指定的元素

```c
fill(iterator beg, iterator end, vale);
fill(开始迭代器 , 结束迭代器, 填充的值);

void test() {
	vector<int> v(10, 0);
	fill(v.begin(), v.end(), 1);
}
```
