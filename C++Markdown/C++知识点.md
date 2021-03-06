# C++知识点
## C++

### 1.const

#### 	C语言中的const：用const修饰的变量仍然是一个变量

1.  指针常量:即指针本身的值是不可改变的，而指针指向的变量的值是可以改变的;

2.  常量指针:即指针指向的变量的值是不可改变的，而指针本身的值是可以改变的;

3.  用const修饰的变量必须在声明时进行初始化；

#### 	C++中的const：用const修饰过后，就变成常量了
1.  指针常量:即指针本身的值是不可改变的，而指针指向的变量的值是可以改变的;

2.  常量指针:即指针指向的变量的值是不可改变的，而指针本身的值是可以改变的;

3. 用const修饰的变量必须在声明时进行初始化；

4.  const 修饰类成员函数时，成员函数内的成员变量是不可以改变的
##### 考点1 const修饰的数据并非一定安全
    mutable修饰成员变量时，可以在const修饰的函数里被改变
    
    ```c++
    void Kf()const{
        ++_cm; // 错误
        ++_ct; // 正确
    }
    private:
    int _cm;
    mutable int _ct;
    ```
    const仅仅是不能通过变量名去操作这块内存，但是可以通过其它方法，如通过指针是可以修改被const修饰的那块内存的。

##### 考点2 const 成员函数
	函数不改变类的成员变量，除非变量被mutable 修饰
```C++
const Type Func(Type Val);
```
##### 考点3 const 与 #define相比不同之处
	const常量有数据类型，而宏常量没有数据类型
	编译器可以对前者进行类型安全检查，而对后者只进行字符替换，没有类型安全检查

### 2.多态
#### 多态类型:
​	静态多态:函数重载和运算符重载属于静态多态

​	动态多态：通过继承重写基类的虚函数实现的多态，**在程序运行时**根据基类的引用（指针）指向的对象来确定自己具体该调用哪一个类的虚函数。 运行时在虚函数表中寻找调用函数的地址。



### 模板

作用：建立通用的模具，提高复用性

模板机制： 函数模板

​					类模板

**函数模板**：建立一个通用函数，其函数返回值类型和形参类型可以不具体制定，用一个虚拟的类型来指代。

语法：

```
template<typename T> //template声明创建模板，typename表明其后的符号是数据类型也可以使用class替代，可用class替代，T通用数据类型
函数声明或定义

调用方式：
1. myswap(a, b);//编译器自动推导
2. myswap<int>(a, b);//显示指定类型
```

例：

```
//函数模板
template<typename T> //声明一个模板，告诉编译器后面代码中紧跟着的T不要报错，T是一个通用数据类型

void myswap(T &a,T &b) {
	T temp;
	temp = a;
	a = b;
	b = temp;
}

int main() {
	int a = 10;
	int b = 9;
	cout << "交换前" << endl;
	cout << "a =" << a << endl;
	cout << "b =" << b << endl;
	myswap(a, b);//编译器自动推导
	cout << "交换后" << endl;
	cout << "a =" << a << endl;
	cout << "b =" << b << endl;

	
	cout << "交换前" << endl;
	cout << "a =" << a << endl;
	cout << "b =" << b << endl;
	myswap<int>(a, b);//显示指定类型
	cout << "交换后" << endl;
	cout << "a =" << a << endl;
	cout << "b =" << b << endl;
	system("pause");
	return 0;
}
```



函数模板注意事项

​		1.自动类型推导，必须推导出一致的数据类型T才可以使用

​		2.模板必须要确定出T的数据类型才可以使用

函数模板有时候无法处理一些自定义类型，但是可以利用具体化自定义类型的版本实现代码，优先被调用

例：

```
template<class T>//原模块
bool myCompare(T a, T b) {
	return a > b;
}
 //具体化person版本
template<> bool myCompare(person &p1, person &p2) {
	return (p1.name == p2.name && p1.age == p2.age);
}

```

###### 类模板

作用:建立一个通用类，类中的成员数据类型可以不具体制定，用一个虚拟的类型来代表。

语法：

```
template<typename T>
类
```

例：

```
template<class nametype,class agetype>
class person {
public:
	nametype name;
	agetype age;

	person(nametype n, agetype a) {
		this->age = a;
		this->name = n;
	}

};
```

函数模板和类模板的区别

|          | 函数模板                                                     | 类模板                                                       |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 调用方式 | myswap(a, b);//编译器自动推导<br />myswap<int>(a, b);//显示指定类型 | person<string, int> p1("dsfds", 6666);//显示制定类型         |
|          |                                                              | 可以指定默认类型：template <class Nametype,class Agetype = int> |

类模板对象做函数参数
		1，指定传入类型 -- 直接显示函数传参方式
		2，参数模板化   -- 将对象中的参数变为模板进行传递
		3，整个类模板化 -- 将这个对象类型模板化进行传递

```
void printPerson1(person<string, int>& p) {
	p.showperson();
}

template <class T1, class T2>
void printPerson2(person<T1, T2> &p) {
	p.showperson();
	cout << "T1的类型为:" << typeid(T1).name() << endl;
	cout << "T2的类型为:" << typeid(T2).name() << endl;
}

template<class T>
void printPerson3(T& p) {
	p.showperson();
}

void test() {
	person <string> p("唐山",23);
	printPerson1(p);
	printPerson2(p);
	printPerson3(p);
}
```

类模板与继承
注意要点:
		1，当子类继承的父类是一个类模板时，子类在声明的时候，要指定父类中T的类型，如果不指定，编译器无法给子类分配内存
		2，子类变为类模板时，可灵活指定父类中T的类型

```
template<class T>
class Base {
public:
	T a;
};

class Son :public Base<int> {
};

template<class T1,class T2>
class Son2 :public Base<T1> {
public:
	Son2() {
		cout << typeid(T1).name() << endl;
		cout << typeid(T2).name() << endl;
	}
};

void test1() {
	Son s;
	char a = 's';
	Son2<char, int> s2;
}
```

类模板成员函数类外实现：需要加上模板参数列表

```
template <class T>
class TV {
public:
	T size;
	TV(T size);
	void watchTV();
};

//构造函数
template<class T>
TV<T>::TV(T) {
	this->size = size;
}
//成员函数
template<class T>
void TV<T>::watchTV() {
	cout << "一起来看电视啊" << endl;
}
```



### STL 

```
C++的面向对象和泛型编程思想目的就是复用性的提升，STL是一套数据结构和算法的标准，用来减少重复性工作。
```

##### STL基本概念

STL(Standard Template Library,标准模板库)

STL从广义上分为：容器，算法，迭代器

容器和算法之间通过迭代器进行无缝连接。

STL几乎所有的代码都采用了模板类或者模板函数

##### STL六大组件

STL大致分为六大组件：容器，算法，迭代器，仿函数，适配器(配接器)，空间配置器

1. 容器：各种数据结构，如vector，list，deque，set，map等，用来存放数据。

2. 算法：各种常用的算法，如sort，find，copy，for，each等

3. 迭代器：扮演了容器与算法之间的胶合剂

4. 仿函数：行为类似函数，可作为算法的某种策略

5. 适配器：一种用来修饰容器或者仿函数或者迭代器接口的东西。

6. 空间适配器：负责空间的配置与管理

##### STL中的容器，算法，迭代器

###### 容器

​	将常用的数据结构实现出来:数组，链表，树，栈，队列，集合，映射表等

​	容器分类：

1. 序列式容器：强调值的排序，序列式容器中的每个元素均有固定的位置。
2. 关联式容器：二叉树结构，各元素间没有严格物理上的顺序关系。

###### 算法（algorithms）

​	有限的步骤，解决逻辑上或数学上的问题

​	算法分类:

1. 质变算法:指运算过程中会更改区间内的元素内容，例如拷贝，替换，删除等等。
2. 非质变算法:是指运算过程中不会更改区间内的元素内容，例如查找，计数，遍历，查找极值等等。

###### 迭代器

提供一种方法，使之能够依序寻访某个容器所含的各个元素，而又无需暴露该容器的内部表示方式。

每个容器都有专属的迭代器。

迭代器种类

| **输入迭代器**     | **对数据的只读访问**                                   | **只读，支持++，==，！=**          |
| ------------------ | ------------------------------------------------------ | ---------------------------------- |
| **输出迭代器**     | **对数据的只写访问**                                   | **只写，支持++**                   |
| **前向迭代器**     | 读写操作，并能向前推进迭代器                           | 读写，支持++，==，！=              |
| **双向迭代器**     | 读写操作，并能向前和向后操作                           | 读写，支持++，--                   |
| **随机访问迭代器** | 读写操作，可以以跳跃方式访问任意数据，功能最强大迭代器 | 读写，支持++，--，[n],-n,<,<=,>,>= |

常用的迭代器为双向迭代器和随机访问迭代器

```
void myPrint(int val) {
	cout << val << endl;
}
// 遍历容器
for_each(v.begin(), v.end(), myPrint);//利用回调的原理进行处理
```



容器 string

1. string 是C++的字符串，本质上是一个类
2. string和char*的区别：	
​   char*是一个指针

​	string是一个类，类内部封装了char\*，是一个char\*类型的容器.
3. string封装了一些成员方法:find 查找，copy 拷贝，delete 删除， replace 替换， insert 插入

4. string构造函数

   ​	string()//无参构造

   ​	string(const char* s)//有参构造 const char * str = “helloworld”;

   ​	string(const string& str)//拷贝构造

   ​	string(int n,char c)//使用n个字符串初始化
   
5. string查找和替换

   ​	int find(const string& str,int pos = 0)const;//查找第一次出现的位置，从pos开始查找。。。

   ​	string& replace(int pos,int n,const string& str);//替换从pos开始n个字符为字符串str

string字符串比较：按照字符串ASCII码进行比较，= 返回1，> 返回1, <返回-1

   ​	int compare(const string &s);与字符串s进行比较

   例:string str1;string str2;

   ​	if(str1.compare(str2)==0){}

6. string字符获取

   ​	char& operator[](int n) //通过[]方式获取字符（str.size()返回字符串长度）,例str[i]

   ​	char&at(int n);//通过st方法获取字符,例:str.at(i)

   ​	修改:

   ​	str[i] = ‘x’;

   ​	str.at(i) = ‘x’;

7. string插入和删除

   插入：str.insert(1,str2)//插入位置，插入字符串

   删除：str.erase(1,3)//从位置1起删除3个字符

8. string截取子串

   substr = str.substr(1,3)//从位置1起截取3个字符

##### 容器 vector

​	功能:vector 数据结构和数组非常相似，也称为单端数组

​	vector和数组区别：数组是静态空间，而vector可以动态扩展

(动态扩展不是在原空间之后续接新空间，而是找更大的空间，然后将原数据拷贝到新空间，释放原空间)

###### vector赋值操作:

​		vector& operator = (const vector &vec);//重载等号操作符 v1 = v2

​		assign(beg,end);//将[beg, end]区间中的数据拷贝赋值给本身 v3 = assign(v1.begin(),v1.end())

​		assign(n,elem);//将n个elem拷贝赋值给本身v4 = assign(4,100) -> [100,100,100,100]



###### vector容量和大小相关操作:

​		empty() //容器是否为空

​		capacity() //容器的容量

​		size() //返回容器中元素的个数

​		resize(int Num) //指定容器长度，若容器变长，则以默认值填充新位置，反之，则超出元素被删除

​		resize(int Num,elem)//同上，只不过默认值为elem

###### vector插入和删除操作：

​		push_back(ele)	//尾部插入元素ele

​		pop_back()	//删除最后一个元素

​		insert(const_iterator pos,ele)	//迭代器指向pos插入ele

​		insert(const_iterator pos,count,ele)	//迭代器指向pos插入count个ele

​		erase(const_iterator pos)  //删除迭代器指向的元素

​		erase(const_iterator start，const_iterator end)  //删除迭代器指向的元素区间

​		clear() 	//删除容器中的所有元素



vector数据存储

​		v.at(idx)  //返回索引idx所指的数据

​		v[idx] //同上

​		v.front()	//返回容器中的第一个元素

​		v.back()	//返回容器中的最后一个元素



vector互换容器

//可以利用容器元素互换收缩容器内存

​		v.swap(vec)	//将vec与本身的元素互换

vector预留空间

​	//减少vector在动态扩展容量时的扩展次数

​		reserve(int len)	//容器预留len个元素长度，预留位置不初始化，元素不可访问

vector底层是怎么实现的？



###### deque 容器

​		功能:双端数组，可以对头部和尾部插入删除

deque和vector区别

​		vector对于头部插入删除效率低，数据量越大，效率越低

​		deque相对而言，头部的插入删除速度更快

​		vector访问员是时速度会比deque快

![](%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210910205746.png)



deque内部工作原理:

deque内部有一个中控器，维护每段缓冲区中的内容，缓冲区中存放真实数据

中控器维护的是每个缓冲区的地址，使 得使用deque时像一片连续的内存空间

![](%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210910210132.png)



deque构造函数和vector的构造函数几乎一致

| deque构造函数                                                | vector构造函数                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| deque<type> d;<br />deque(d.beg,d.end);<br />deque(n,elem);<br />deque(const deque &deq); | vector <type> v<br />vector(v.beg,v.end);<br />vector(n,elem);<br />vector(const vector &vec); |

| deque大小操作                                                | vector大小操作                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| d.empty()<br />d.size()<br />d.resize(num)<br />d.resize(num,elem) | v.empty()<br />v.capacity()<br />v.size()<br />v.resize(num)<br />v.resize(num,elem)<br />v.reserve() |

deque没有容量的概念，所以不需要capacity()

| deque插入和删除                                              | vector插入和删除                                             |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| push_back(elem)<br />push_front(elem)<br />pop_back()<br />pop_front()<br /><br />insert(pos,elem)<br />insert(pos,n,elem)<br />insert(pos,d2.beg,d2.end)<br />clear()<br />erase(d.beg,d.end)<br />erase(pos) | push_back(elem)<br />pop_back()<br /><br />insert(pos,elem)<br />insert(pos,n,elem)<br />insert(pos,d2.beg,d2.end)<br />clear()<br />erase(v.beg,v.end)<br />erase(pos) |

deque双端数组，vector单端数组

| deque访问元素                                  | vector访问元素                                 |
| ---------------------------------------------- | ---------------------------------------------- |
| deq[1]<br />deq.at(1)<br />front()<br />back() | v[idx]<br />v.at(idx)<br />front()<br />back() |

###### stack容器

构造函数：

stack<type> st; 

stack(const stack &st0) st;

赋值操作:

stack& operator=(const stack &st); //重载等号操作符

数据存取:

push(elem);

pop();

top();

大小操作:

empty();	//栈是否为空

size();		//返回栈的大小



###### queue容器：先进先出

构造函数

queue<type> q;

queue(const queue &q0);

赋值操作

queue& operator=(const queue &q);//重载等号操作符

数据存取

push()

pop()

back()

front()

大小操作:

empty();	//栈是否为空

size();		//返回栈的大小

##### list容器

将数据进行链式存储。是物理存储单元上非连续的存储结构，数据元素的逻辑顺序是通过链表中的指针链接实现的

（双向循环链表）

构造函数:

- list<type> lst;

- list(beg,end);

- list(n,elem);

- list(const list &lst);


赋值操作和交换

assign(beg,end)  

assign(n,elem)

list& operator=(const &list lst)

swap(lst)

大小操作

size()

empty()

resize(Num)

resize(Num,elem)

插入删除

- push_front()

- push_back()

- pop_front()

- pop_back()

- insert(pos,elem)

- insert(pos,n,elem)

- insert(pos,beg,end)

- clear()

- erase(beg,end)

- erase(pos)

- remove(elem)

- begin()

- end()


数据存取

front()

back()

反转和排序

reverse()

lst.sort()	//对于自定义类型排序，需要自定义排序规则

优点:1.动态存储分配，不会造成内存浪费和溢出

​		2.链表执行插入删除十分方便

缺点:1.空间(指针域)和时间(遍历)消耗较大

list插入删除操作不会影响list迭代器，但是vector会。



##### list和vector(**顺序表**)优缺点比较

vector:优.和数组类似开辟一段连续的空间，并且支持随机访问，所以它的查找效率高其时间复杂度O(1)。

​			缺.由于开辟一段连续的空间，所以插入删除会需要对数据进行移动比较麻烦，时间复杂度O(n)。

list:	 优.底层实现是循环双链表，当对大量数据进行插入删除时，其时间复杂度O(1)

​			缺.底层没有连续的空间，只能通过指针来访问，所以查找数据需要遍历其时间复杂度O（n），没有提供[]操作符的重载。



###### set/multiset容器

set基本概念：所有元素都会在插入时自动被排序

本质：属于关联式容器，底层结构是用二叉树实现的。

set和multiset的区别:

​		set不允许容器中有重复的元素，但是multiset允许

set构造和赋值:

- set<type> s;

- set(const set& s);

- set& operator=(const set &s);


set大小和交换

- size()

- empty()

- swap(s)


set插入删除

- insert(elem);

- clear()

- erase(pos)

- erase(beg,end)

- erase(elem)


set查找和统计

​		find(key)

​		count(key)

set和multiset区别

​		set不允许容器中有重复的元素，但是multiset不会检测数据，因此可以插入相同数据

​		set insert的时候会返回结果，表示是否插入成功

set容器排序：利用仿函数改变排序规则,而且必须在插入数据区才能更改





pair对组创建

​		pair<type,type> p(value1,value2);

​		pair<ttype,type> p = make_pair(value1,value2);



map/multimap容器

基本概念:

- map中所有元素都是pair
- pari中两个元素分别为key值和value值
- 所有元素都会根据元素的键值自动排序

本质：

​	map/multimap属于关联式容器，底层结构和set/multiset一样都是用二叉树实现的

优点：可以根据key值快速找到value值

map和multimap区别：map不允许容器中有重复的key，而multimap允许。



map构造和赋值

- map<T1,T2> m;
- map<const map &m>;//拷贝构造
- map& operator=(const map &m)

map大小和交换

- size()
- empty()
- swap()

map插入和删除

- insert(elem)
- clear()
- erase(pos)//删除pos迭代器所指的元素，返回下一个元素的迭代器
- erase(beg,end)//删除(beg,end)区间的所以元素，返回下一个迭代器
- erase(key)

map查找和统计

find(key)//返回对应元素迭代器，否则返回m.end()

count(key)

map自定义排序可以利用仿函数实现



|        |              |                                                              |
| ------ | ------------ | ------------------------------------------------------------ |
| string |              |                                                              |
| vector | 单端数组     | 高效的随机访问和在尾端插入/删除操作<br />其他位置的插入/删除操作效率低下 |
| deque  | 双端数组     | 内部方便插入和删除操作方便<br />随机访问方便<br />占用内存多 |
| queue  | 队列         | queue只能从队首删除元素,但是两端都能访问                     |
| list   | 双向循环链表 | 动态存储分配，不会造成内存浪费和溢出<br />链表执行插入删除十分方便<br />空间(指针域)和时间(遍历)消耗较大 |
| set    | 二叉树       |                                                              |
| map    | 二叉树       |                                                              |

##### 函数对象（仿函数）

本质:函数对象使用重载的()是，行为类似函数调用

特点：

- 使用仿函数时，行为类似函数调用，可以有参数和返回值
- 可以有对象本身的参数和方法

谓词

概念：

- 返回bool类型的仿函数称为谓词
- 接收n个参数，被叫做n元谓词

```
class GreateFive {
public:
	bool operator()(int val) {
		return val > 5;
	}
};

int main() {
	vector<int> v5;
	for (int i = 2; i < 22; i += 3) {
		v5.push_back(i);
	}

	vector<int>::iterator it = find_if(v5.begin(), v5.end(), GreateFive());
	if (it == v5.end()) {
		cout << "全小于5" << endl;
	}
	else {
		cout << "存在大于5" << *it<< endl;
	}
	return 0;
}
```

ps:留意find_if函数使用



##### 内建仿函数#include<functional>

分类：

| 算术                                   | 关系                                          | 逻辑                                   |
| -------------------------------------- | --------------------------------------------- | -------------------------------------- |
| template<class T> plus<T> //加法       | template<class T> equal<T> //等于             | template<class T> logical__and<T> //与 |
| template<class T> minus<T> //减法      | template<class T> not_equal<T> //不等于       | template<class T> logical__or<T> //或  |
| template<class T> multiplies<T> //乘法 | template<class T> greater<T> //大于           | template<class T> logical__not<T> //非 |
| template<class T> dixides<T> //除法    | template<class T> greater_equal<T> //大于等于 |                                        |
| template<class T> modulus<T> //取模    | template<class T> less<T> //小于              |                                        |
| template<class T> nagate<T> //取反     | template<class T> less_equal<T> //小于等于    |                                        |

例：

```
negate<int> n;
cout << n(50) << endl;
plus<int> p;
cout << p(10, 20) << endl;
```

```
sort(v5.begin(), v5.end(), greater<int>());//逆序
```

```
transfrom(v.begin(),v.end(),v2.begin(),logical_not<bool>());//取反后迁移
```

##### STL常用算法:头文件：<algorithm><functional><numeric>

| 算法             | 例子                                                         |                                                    |
| ---------------- | ------------------------------------------------------------ | -------------------------------------------------- |
| for_each         | for_each(iterator beg,iterator end,func)//将遍历的数据当做参数传入func | 遍历容器，函数func为函数名，仿函数需要加上()       |
| transform        | transform(v.begin(),v.end(),v2.begin(),_func)//目标容器需要提前开辟足够空间 | 搬运容器到另一个容器中                             |
| find             | fing(iterator beg,iterator end,value)//查找自定义类型时需要重载== | 查找指定元素，返回指定元素迭代器或者end()          |
| find_if          | fing_if(iterator beg,iterator end,_Perd)//_Perd为函数或者谓词 | 按条件查找元素(只返回符合条件的第一个元素的迭代器) |
| adjacent_find    | adjacent_fing(iterator beg,iterator end)//返回第一个位置迭代器或者end() | 查找重复相邻的元素                                 |
| binary_search    | binary_search(iterator beg,iterator end，value)//元素必须有序,返回bool | 二分查找法                                         |
| count            | count(iterator beg,iterator end，value)//自定义类型需要重载== | 统计元素个数                                       |
| count_if         | count_if(iterator beg,iterator end，_Perd)//_Perd为函数或者谓词 | 按条件统计元素个数                                 |
| sort             | sort(iterator beg,iterator end,_Pred)                        | 对容器内元素进行排序                               |
| random_shuffle   | random_shuffle(iterator beg,iterator end)//记得加上随机数种子 | 洗牌，指定范围内的元素随机调整次序                 |
| merge            | merge(iterator beg1,iterator end1,iterator beg2,iterator end2,iterator dest)//dest目标容器开始迭代器,目标容器必须提前开辟空间 | 容器元素合并，并存储到另一容器中                   |
| reverse          | reverse(iterator beg,iterator end)                           | 反转元素                                           |
| copy             | copy(v1.begin(),v1.end(),v2.begin())                         | 将容器内指定区间内元素拷贝到另一容器中             |
| replace          | replace(iterator beg,iterator end,oldval,newval )            | 将容器指定区间内旧元素替换为新元素                 |
| replace_if       | replace(iterator beg,iterator end,_pred,newval )             | 将容器指定区间内满足条件的元素替换为新元素         |
| swap             | swap(container C1,container C2)//同种容器                    | 互换两容器元素                                     |
| accumulate       | accumulate(iterator beg,iterator end,value )//value 为起始值，仅求累加，value = 0 | 计算容器元素累计总和+value                         |
| fill             | fill(iterator beg,iterator end,value )//value为填充值        | 向容器中填充元素                                   |
| set_intersection | set_intersection(iterator beg1,iterator end1,iterator beg2,iterator end2,iterator dest)//dest目标容器开始迭代器,目标容器必须提前开辟空间 | 交集                                               |
| set_union        | set_union(iterator beg1,iterator end1,iterator beg2,iterator end2,iterator dest)//dest目标容器开始迭代器,目标容器必须提前开辟空间 | 并集                                               |
| set_difference   | set_difference(iterator beg1,iterator end1,iterator beg2,iterator end2,iterator dest)//dest目标容器开始迭代器,目标容器必须提前开辟空间 | 差集                                               |



## 问题

##### 1. static关键字的作用

​	

|          | 静态成员变量（面向对象）                                     | 静态成员函数（面向对象）                                     | 静态全局变量（面向过程） | 静态局部变量（面向过程）                                     | 静态函数（面向过程）                           |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------ | ------------------------------------------------------------ | ---------------------------------------------- |
| 内存分配 | 编译时在静态(全局)区分配，程序结束时释放；sizeof计算时不会包含静态成员变量 |                                                              | 全局区                   | 全局区                                                       |                                                |
| 作用域   |                                                              | 通过对象名或者类名访问                                       |                          | 静态局部变量始终驻留在全局数据区，直到程序运行结束。但其作用域为局部作用域，当定义它的函数或语句块结束时，其作用域随之结束 | 只能在声明它的文件当中可见，不能被其它文件使用 |
| 访问限制 |                                                              | 静态成员函数不能访问非静态成员函数和非静态成员变量；非静态成员函数可以任意地访问静态成员函数和静态数据成员 |                          |                                                              |                                                |
| 其他     | 静态成员变量没有进入程序的全局命名空间，因此不存在与程序中其它全局命名冲突的可能 | 它不具有this指针；                                           |                          |                                                              |                                                |



##### 2.析构函数为虚函数的作用，解释原理；构造函数里面调用虚函数执行的是父类还是子类，为什么。

​		虚析构函数使得在删除指向子类对象的基类指针时可以调用子类的析构函数达到释放子类中堆内存的目的，而防止内存泄露。

​		不要在构造函数和析构函数中调用虚函数，这样调用跟调用普通函数是一样的，只会调用当前类的函数。构造函数和析构函数，都会把虚函数表指针设置为当前类的虚函数表地址，因此，在构造函数和析构函数中调用的虚函数，都是调用的当前类的函数。

##### 3.assert的作用。

​		**作用**:原型定义在<assert.h>中，其作用是如果它的条件返回错误，先向stderr打印一条出错信息，然后通过调用 abort 来终止程序运行。

​		**缺点**:已放弃使用assert()的缺点是，频繁的调用会极大的影响程序的性能，增加额外的开销。在调试结束后，可以通过在包含#include <assert.h>的语句之前插入 #define NDEBUG 来禁用assert调用。

##### 4.内存对齐，对齐的意义。

​		Sizeof(B)为8字节的整数倍。

​		用空间效率来换时间效率。

##### 5.类之间的关系有哪些。举例现实中的事物说明。

​		继承（父子），组合(不可分割整体)，聚合(可分割整体)，

​		泛化 = 实现 > 组合 > 聚合 > 关联 > 依赖

##### 6.面向对象的设计原则有哪些。

|                    |                                                              |
| ------------------ | ------------------------------------------------------------ |
| **单一职责原则**   | 一个类，最好只做一件事                                       |
| **开放封闭原则**   | 软件实体应该是可扩展的，而不可修改的                         |
| **Liskov替换原则** | 子类可以替换基类，但是基类不一定能替换子类。拓展性,继承复用  |
| **依赖倒置原则**   | 具体而言就是高层模块不依赖于底层模块，二者都同依赖于抽象；抽象不依赖于具体，具体依赖于抽象。 |
| **接口隔离原则**   | 使用多个小的专门的接口，而不要使用一个大的总接口             |
| **合成复用原则**   | 尽量使用合成/聚合的方式，而不是使用继承                      |

##### 7.用过哪些设计模式，详细讲一下观察者模式。

###### 工厂模式

###### 观察者模式

​		作用:定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

​		使用方式:在抽象类里有一个 ArrayList 存放观察者们。

​		实例： 1、拍卖的时候，拍卖师观察最高标价，然后通知给其他竞价者竞价。

​		优点：1、观察者和被观察者是抽象耦合的。 2、建立一套触发机制。

​		缺点：1、如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。 2、如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。 3、观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。

###### MVC 模式

###### 适配器模式

​		作用:是作为两个不兼容的接口之间的桥梁

​		案例:读卡器是作为内存卡和笔记本之间的适配器

###### 装饰器模式

​		功能:允许向一个现有的对象添加新的功能，同时又不改变其结构。这种类型的设计模式属于结构型模式，它是作为现有的类的一个包装。

​		案例:不论一幅画有没有画框都可以挂在墙上，但是通常都是有画框的，并且实际上是画框被挂在墙上。在挂在墙上之前，画可以被蒙上玻璃，装到框子里；这时画、玻璃和画框形成了一个物体。





##### 8.malloc和new的区别

![](%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210913115816.png)

new分配内存到**自由存储区**，不仅可以是堆，还可以是静态存储区，这都看operator new在哪里为对象分配内存



##### 9.面向对象和面向过程的区别

​	**面向过程**是一种以**过程**为中心的编程思想，它首先分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，在使用时依次调用，是一种基础的顺序的思维方式。

​	面向对象是按人们 认识客观世界的系统思维方式，采用基于对象（实体）的概念建立模型，模拟客观世界分析、设计、实现软件的编程思想，通过面向对象的理念使计算机软件系统能与现实世界中的系统一一对应。

##### 10.sizeof 与 strlen的区别

​		sizeof 是运算符，strlen 是函数。

​		sizeof 可以用类型做参数，**strlen** 只能用 **char\*** 做参数，且必须是以 **\0** 结尾的

​		数组做 **sizeof** 的参数不退化，传递给 **strlen** 就退化为指针了

​		大部分编译程序在编译的时候就把 **sizeof** 计算过了，是类型或是变量的长度，这就是 **sizeof(x)** 可以用来定义数组维数的原因；strlen 的结果要在运行的时候才能计算出来，是用来计算字符串的长度，不是类型占内存的大小。

```
char str[20]="0123456789";
int a=strlen(str); // a=10;
int b=sizeof(str); // 而 b=20;
```



### 操作系统

##### 1.缓存算法（FIFO 、LRU、LFU三种算法的区别）

FIFO ：先进先出（队列）

LRU：最近最久未使用算法（空间满时，最久没有访问的数据最先被置换（淘汰））

LFU：最近最少使用算法（当空间满时，最小频率访问的数据最先被淘汰。）

##### 2.缺页中断算法(FIFO,LRU)

缺页本身是一种中断，与一般的中断一样，需要经过4个处理步骤： 
　　1. 保护CPU现场 
　　2. 分析中断原因 
　　3. 转入缺页中断处理程序进行处理 
　　4. 恢复CPU现场，继续执行 

FIFO ：置换最先调入内存的页面，即置换在内存中驻留时间最久的页面。

LRU：最近最久未使用置换算法



##### 3.进程的几种状态，对内核对象的了解，用过哪些，有什么注意事项

就绪:等CPU

运行:

阻塞:等IO资源



​		内核，是一个操作系统的核心。是基于硬件的第一层软件扩充，提供操作系统的最基本的功能，是操作系统工作的基础， 它负责管理系统的进程、内存、设备驱动程序、文件和网络系统，决定着系统的性能和稳定性。
​        现代操作系统设计中，为减少系统本身的开销，往往将一些与硬件紧密相关的（如中断处理程序、设备驱动程序等）、基本的、公共的、运行频率较高的模块（如时钟管理、进程调度等）以及关键性数据结构独立开来， 使之常驻内存，并对他们进行保护。    
​        也就是说：内核是操作系统进行管理的一块区域（内存），对于应用程序是不可见的。

 内核对象 就是在操作系统内核中进行资源分配和管理的一种数据结构。



##### 4.进程、线程、协程的区别

**进程**：

​		1、进程是系统进行资源分配的一个独立单位。

​		2、每个进程都有自己的独立内存空间，不同进程通过进程间通信来通信。

​		3、由于进程比较重量，占据独立的内存，所以上下文进程间的切换开销（栈、寄存器、虚拟内存、文件句柄等）比较大，但相对比较稳定安全。

**线程**：

​		1，线程是进程的一个实体,是CPU调度的基本单位,它是比进程更小的能独立运行的基本单位。它与同属一个进程的其他的线程共享进程拥有的全部资源。

​		2、线程间通信主要通过共享内存，上下文切换很快，资源开销较少，但相比进程不够稳定容易丢失数据。

**协程**：

​		1、是一种用户态的轻量级线程，拥有自己的寄存器上下文和栈，协程的调度完全由用户控制。

​		2、一个线程可以多个协程，一个进程也可以单独拥有多个协程。

​		3、线程进程都是同步机制，而协程则是异步。

​		4、协程能保留上一次调用时的状态，每次过程重入时，就相当于进入上一次调用的状态。

### 计算机网络

##### 1.TCP三次挥手四次握手的过程

​		1、TCP通信过程：建立TCP连接通道，传输数据，断开TCP连接通道

​		2、三次握手建立连接：

​				客户端发送syn包(seq=x)到服务器，并进入SYN_SEND状态，等待服务器确认

​				服务器收到syn包，必须确认客户的SYN(ack=x+1)，同时自己也发送一个SYN包(seq=y)，即SYN+ACK包，此时服务器进入SYN_RECV状态

​				客户端收到服务器的SYN+ACK包，向服务器发送确认包ACK(ack=y+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态

​		3、传输数据：

​				超时重传机制：用来保证TCP传输的可靠性。

​				快速重传：接受数据一方发现有数据包丢了就会发送ack报文告诉发送端重传丢失的报文。比较超时重传和快速重传：超时重传是发送端在傻等超时，然后触发重传;快速重传则是接收端主动告诉发送端数据没收到，然后触发发送端重传。

​				流量控制(TCP滑动窗流量控制)：TCP头里有一个字段叫Window，是接收端告诉发送端还有多少缓冲区可以接收数据。发送端就可以根据这个接收端的处理能力来发送数据，而不会导致接收端处理不过来。 滑动窗可以是提高TCP传输效率的一种机制。

​				拥塞控制滑动窗用来做流量控制:流量控制只关注发送端和接受端自身的状况，而没有考虑整个网络的通信情况。拥塞控制，则是基于整个网络来考虑的。主要包括：慢启动，拥塞避免，拥塞发生，快速恢复。

​		4、四次握手:

​				主动关闭方发送一个FIN，用来关闭主动方到被动关闭方的数据传送。也就是主动关闭方告诉被动关闭方：我已经不会再给你发数据了.

​				被动关闭方收到FIN包后，发送一个ACK给对方，确认序号为收到序号+1.

​				被动关闭方发送一个FIN，用来关闭被动关闭方到主动关闭方的数据传送，也就是告诉主动关闭方，我的数据也发送完了，不会再给你发数据了。

​				主动关闭方收到FIN后，发送一个ACK给被动关闭方，确认序号为收到序号+1。

##### 2.TCP为什么三次挥手四次握手

​		三次挥手:确认双方收发功能都正常，四次也可以但是显得比较多余

​		四次挥手:分别对应主动方关闭发送数据，被动方确认；被动方关闭发送数据，主动方确认。





### C++网络编程

##### TCP/IP基础

###### ISO/OSI模型



###### TCP/IP四层模型



##### socket编程

##### 进程间通信

##### 线程

