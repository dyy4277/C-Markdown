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

##### 考点2 const 成员函数mutable 修饰
	函数不改变类的成员变量，除非变量被
```C++
const Type Func(Type Val);
```
##### 考点3 const 与 #define相比不同之处
	const常量有数据类型，而宏常量没有数据类型
	编译器可以对前者进行类型安全检查，而对后者只进行字符替换，没有类型安全检查

### 2.多态
#### 多态类型:
​	静态多态:函数重载和运算符重载属于静态多态



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

​		v.end()	//返回容器中的最后一个元素



vector互换容器

//可以利用容器元素互换收缩容器内存

​		v.swap(vec)	//将vec与本身的元素互换





vector预留空间

​	//减少vector在动态扩展容量时的扩展次数

​		reserve(int len)	//容器预留len个元素长度，预留位置不初始化，元素不可访问

###### deque 容器

​		功能:双端数组，可以对头部和尾部插入删除

deque和vector区别

​		vector对于头部插入删除效率低，数据量越大，效率越低

​		deque相对而言，头部的插入删除速度更快

​		vector访问员是时速度会比deque快

![](%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210910205746.png)



deque内部工作原理:

deque内部有一个中控器，维护每段缓冲区中的内容，缓冲区中存放真实数据

中控器维护的是每个缓冲区的地址，使得使用deque时像一片连续的内存空间

![](%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20210910210132.png)