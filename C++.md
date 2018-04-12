# C++

## 引用

整型引用：```int &b = a```

指针类型引用：类型 *&指针引用名 = 指针

```
#include <iostream>
using namespace std;
int main(void)
{
    int a = 10; //给a一个地址，并在该地址中存储a的值10
    int *p = &a; //&为取地址符，定义p变量，赋予另一个地址，该地址中存储a的地址
    int *&q = p; //定义q变量，地址符p的地址，存储a的地址
    *q = 20; //访问q地址下的值，获得a的地址，再访问a的地址，对a值进行修改
}
```



引用作函数参数：

```
void fun(int &a, int &b) //将输入的两个参数，用别名a，b分别表示且能直接修改原数值
{
    int c = 0;
    c = a;
    a = b;
    b = c;
}

int x = 10, y = 20;
fun(x, y);
```



## const

const与 基本数据类型：在定义前加上const，如：```const int x = 3；```，那么，再对x的值做任何更改都会报错

const与指针类型：写法为：```const int *p = NULL```或```int const *p = &x```这表示*p是不变的，即```p = 4```将报错

若写为```int * const p = &x```此时表示p所指向的地址是不变的，只能指向x，即```p = &y```将报错



const与引用：``int x = 3; const int &y = x;``此时，对x修改是正确的，但通过y来对x修改是错误的



##函数特性

函数参数默认值：有默认参数值的参数必须写在参数列表的最右端；无实参则用默认值，否则实参将覆盖默认值

函数重载：函数名相同，但函数参数个数或者类型不同

内联函数：

​	关键字：inline

​	特性：1.由编译器决定是否内联编译；

​		    2.内联函数必须逻辑简单，调用频繁的函数建议使用内联函数

​		    3.递归函数无法使用内联方式



## 内存管理

使用new申请内存，使用delete释放内存；申请内存需要判断是否成功，释放内存需要设空指针；在C++中new与delete配套使用



##访问限定符

public；protected；private



## 栈访问对象

对一个类，在栈中创建对象不需要主动回收内存，且对象成员访问如下：

```
int main(void)
{
    TV tv;
    tv.type = 0;
    tv.changeVol();
    return 0;
}
```



## 堆访问对象

对一个类，在堆中创建对象需要申请内存，且使用完成后需要将该内存主动回收，访问成员方式如下：

```
# 创建对象及对对象成员的访问
int main(void)
{
    TV *p = new TV();
    p -> type = 0;
    p -> changeVol();
    delete p;
    p = NULL;
    return 0;
}

# 创建数组对象并访问
int main(void)
{
    TV *p = new TV[5];
    for(int i=0; i<5; i++)
    {
        p[i] -> type = 0;
        p[i] ->changeVol();
    }
    delete []p;
    p = NULL;
    return 0;
}
```



## string

### 初始化

```
string s1; //s1为空串
string s2("ABC"); //用字符串字面值初始化s2
string s3(s2); //将s3初始化为s2的一个副本
string s4(n, 'c'); //将s4初始化为'c'的n个副本
```



## 初始化

### 构造函数

构造函数在对象实例化时被自动调用，且仅被调用一次；构造函数没有返回值；构造函数与类同名；构造函数可以有多个重载形式，但需要满足重载的要求；实例化对象时仅用到一个构造函数；当用户没有 定义构造函数时，编译器将自动生成一个构造函数

#### 无参构造函数

```
class Student
{
    public:
    	Student(){m_strNanme = "jim";} //构造函数为Student()
    private:
    	string m_strName;
};
```

#### 有参构造函数

```
class Student
{
    public:
    	Student(string name)
    	{m_strName = name;}
    private:
    	string m_strName;
};
```

#### 重载构造函数

```
class Student
{
    public:
    	Student(){m_strName = "jim";}
    	Student(string name)
    	{m_strName = name;}
    private:
    	string m_strName;
};
```

#### 初始化列表

初始化列表先于构造函数执行；初始化列表只能用于构造函数；初始化列表可以同时初始化多个数据成员

作用在于能够将构造函数中一些```const```的常量进行初始化

```
class Student
{
    public:
    	Student()"m_strName("Jim"),m_iAge(10){}
    private:
    	string m_strName;
    	int m_iAge;
};
```

#### 拷贝构造函数

如果没有自定义的拷贝构造函数则系统自动生成一个默认的拷贝构造函数

当采用直接初始化或复制初始化实例对象时，系统自动调用拷贝构造函数

定义格式：类名（const 类名&变量名），如：

```
class Student
{
    public:
    	Student(){m_strName = "jim";}
    	Student(const Student&stu){} //在{}中输入拷贝构造函数所需编写内容
    private:
    	string m_strName;
};
```



## 析构函数

如果没有自定义的析构函数则系统自动生成；析构函数在对象销毁时自动调用；析构函数没有返回值、没有参数也不能重载

定义格式：```~类名()```



## 对象数组

定义方法有两种，一种在栈上定义，另一种在堆上定义，如

```
int main(void)
{
    Coordinate coord[3];
    coord[1].m_iX = 10;
    Coordinate *p = new Coordinate[3];
    p[0].m_iY = 20;//或者为p -> m_iY = 20;
    delete[]p;
    p = NULL;
    return 0;
}
```

对于堆上定义的对象数组，在赋值时需要每次改变指针位置，如```p++```，但在输出的时候由于此时指针指向最后一个位置，输出后指针应该进行```p--```操作，但全部打印输出完成后，由于指针指向了非法位置，则需要再进行```p++```操作，使指针指向第一个元素，此时才能进行```delete []p/*由于在创建数组的时候，执行了三次构造函数，若再删除操作时不加[]，则只会删除第一个指针*/```操作



## 深拷贝与浅拷贝

在浅拷贝中，当拷贝的信息中存在堆时，由于对两个对象都要进行销毁操作，但指针指向的位置是同一个，将导致重复操作，电能将崩溃

为了避免，可以编写如下代码：

```
#深拷贝
class Array
{
    public:
    	Array(){m_iCount = 5; m_pArr = new int[m_iCount];}
    	Array(const Array& arr){
            m_iCount = arr.m_iCount;
            m_pArr = new int[m_iCount];
            for(int i=0; i<m_iCount; i++)
            {m_pArr[i] = arr.m_pArr[i];}
    	}
    private:
    	int m_iCount;
    	int *m_pArr;
};
```



## 对象指针

将指针作为对象，进行赋值等操作，如：

```
int main(void)
{
    Coordinate *p = new Coordinate;
    p -> m_iX = 10; //(*p).m_iX = 10;
    p -> m_iY = 20; //(*p).m_iY = 20;
    delete p;
    p = NULL;
    return 0;
}
```

堆中指针的创建有两种方法，如：

```
#1.
Coordinate *p1 = NULL;
p1 = new Coordinate;
#2.
Coordinate *p2 = new Coordiante();
```



## this 指针

当使用与数据成员同名的参数时可以使用this指针来标记处数据成员，如```this->len```



## 常成员函数

在常成员函数中修改数据成员的值将报错

常成员函数如```void printInfo() const```



## 对象常指针和常应用

```
int main(void)
{
    Coordinate coor1(3, 5);
    const Coordinate &coor2 = coor1; //对象常引用，等于给coor1取了别名coor2
    const Coordinate *pCoor = &coor1; //对象常指针
    coor1.printInfo();
    coor2.getX();
    pCoor -> getY();
    return 0;
}
```



## 继承

```
# 公有继承
class Worker:public Person //Person为基类或父类，Worker为派生类或子类
{
    public:
    	void work();
    	int m_iSalary;
};
```

### 公有继承

```c++
class A : public B
```

### 保护继承

```c++
class A : public B
```

### 私有继承

```c++
class A : public B
```

| 基类成员访问属性 | 继承方式 | 派生类成员访问属性 |
| :--------------: | :------: | :----------------: |
|   private成员    |  public  |      无法访问      |
|  protected成员   |  public|     protected      |
|    public成员    |  public |       public       |

| 基类成员访问属性 | 继承方式 | 派生类成员访问属性 |
| :--------------: | :------: | :----------------: |
|   private成员    |  protected  |      无法访问      |
|  protected成员   |  protected  |     protected     |
|    public成员    |  protected  |     protected     |

| 基类成员访问属性 | 继承方式 | 派生类成员访问属性 |
| :--------------: | :------: | :----------------: |
|   private成员    |  private  |      无法访问      |
|  protected成员   |  private  |     private     |
|    public成员    |  private  |     private     |

### 隐藏

在父子关系中，成员同名时就会发生隐藏；此时需要说明父类名称才能调用父类中的同名函数，否则是子类中的同名函数，如：

```c++
int main(void)
{
    Soldier soldier;
    soldier.play();
    soldier.Person::play();
    return 0;
}
```



## 虚析构函数

当父类指针指向堆中子类对象，且需要在父类中释放子类内存时使用，如

```c++
virtual ~Person();
```



## 函数传入参数问题

```c++
1.
    test1(Person p); //此时会对传入的对象创建临时对象，当使用完成后将销毁临时对象
2.
    test2(Person &p); //此时只是会对传入对象产生一个别名，不会产生临时对象，不会产生析构函数的使用
3.
    test3(Person *p); //使用效果与test2相同
```



## 多重继承和多继承

2继承1,3继承2的继承关系称为多重继承

3同时继承1和2的继承关系称为多继承



## 虚继承

```c++
class Worker: virtual public Person
{
};
```



## 宏定义

用于解决重定义

需要在被多继承的文件中添加如下代码：

```c++
#ifndef <文件名>
#define <文件名>

#endif
```



## 虚函数

指相同对象收到不同消息或不同对象收到相同消息时产生不同的动作

即需要添加```virtual```来将函数转化为虚函数，这样就能通过父类定义的指针调用子类的函数



## 虚析构函数

由于当父类指针指向子类对象，使用完成需要销毁时，```delete```父类指针只能释放父类指针中申请的内存，而对于子类对象申请的内存无法释放，此时需要使用虚析构函数

利用```virtual```修饰析构函数，如

```c++
class Shape
{
    public:
    	shape();
    	virtual double calcArea();
    	virtual ~Shape();
}
```



## virtual使用限制

```c++
1.#普通函数不能是虚函数，如
virtual int test()
{}
2.#静态成员函数不能是虚函数，如
class Animal
{
    public:
    	virtual static int getCount();
};
3.#内联函数不能是虚函数，此时inline将失效如
class Animal
{
    public:
    	inline virtual int eat()
        {
            
        }
};
4.#构造函数不能是虚函数，如
class Animal
{
    public:
    	virtual Animal()
        {
            
        }
};
```



## 纯虚函数

没有函数体且在函数名后需要加“=0”；含有纯虚函数的类称为抽象类；抽象类无法实例化对象；抽象类的子类也可能是抽象类；抽象类只有当所有的纯虚函数定义方法后才能实例化对象

```c++
class Shape
{
    public:
    	virtual double calcArea() //虚函数
        {return 0;}
    	virtual double calcPerimeter() = 0; //纯虚函数
    	……
}；
```



## 接口类

在类中仅含有纯虚函数的类称为接口类



## RTTI

运行时类型识别，如

```c++
void doSomething(Flyable *obj)
{
    obj ->takeoff();
    cout << typeid(*obj).name() << endl; //此时能对*obj类型进行识别，若为鸟，则typeid(*obj).name == Bird.name()
    if(typeid(*obj) == typeid(Bird))
    {
        Bird *bird = dynamic_cast<Bird *>(obj); //此时<>输入的为需要转化成的类型，后面加对象即可
        bird -> foraging();
    }
    obj -> land();
}
```



## dynamic_cast转化限制

1.只能应用于指针和引用的转换

2.要转换的类型中必须包含虚函数

3.转换成功将返回子类的地址，失败将返回NULL



## typeid注意事项

1.type_id返回一个type_info对象的引用

2.如果想通过基类的指针获得派生类的数据类型，基类必须带有虚函数

3.只能获取对象的实际类型



## 异常处理

```c++
//try与catch是一对多的关系，catch(...)能够捕获所有的异常
try
{fun1();}
catch(int)
{}
catch(double)
{}
catch(...)
{}
```



## 友元

###友元函数

关键字```friend```

```c++
1.友元全局函数
class Coordinate
{
    friend void printXY(Coordinate &c);
    public:
    	Coordinate(int x, int y);
    private:
    	int m_iX;
    	int m_iY;
};
2.友元成员函数
class Coordinate
{
    friend void Circle::printXY(Coordinate &c);
    public:
    	Coordinate(int x, int y);
    private:
    	int m_iX;
    	int m_iY;
};
```



### 友元类

```c++
class Circle; //需要先在此处声明，不然friend Circle会报错
class Coordinate
{
    friend Circle;
    public:
    	Coordinate(int x, int y);
    private:
    	int m_iX;
    	int m_iY;
};
```



## 友元的注意事项

1.友元关系不可传递

2.友元关系的单向性

3.友元声明的形式及数量不受限制



## 静态注意事项

1.静态数据成员必须单独初始化

2.静态成员函数不能调用非静态成员函数和非静态数据成员

3.静态数据成员只有一份，且不依赖对象而存在



## 一元运算符重载

### 成员函数重载

```c++
#取反(-)
class Coordinate
{
    public:
    	Coordinate(int x, inty);
    	Coordinate& operator-();
    private:
    	int m_iX;
    	int m_iY;
};

#(++)运算符
class Coordinate
{
    public:
    	Coordinate(int x, inty);
    	Coordinate& operator++(); //前置++
    private:
    	int m_iX;
    	int m_iY;
};
#实现
Coordinate &Coordinate::operator-()
{
    m_iX = -m_iX;
    this -> m_iY = -this -> m_iY;
    return *this;
}

class Coordinate
{
    public:
    	Coordinate(int x, inty);
    	Coordinate operator++(int); //后置++，int用来标识此时用于后置++情况;此时返回++之前的值，再次执行才会返回++一次之后的值
    private:
    	int m_iX;
    	int m_iY;
};
#实现
Coordinate Coordinate::operator++(int)
{
    Coordinate old(*this);
    this -> m_iX;
    this -> m_iY;
    return old;
}
```



### 友元函数重载

```c++
class Coordinate
{
    friend Coordinate& operator-(Coordinate &coor);
    public:
    	coordinate(int X, int Y);
    private:
    	int m_iX;
    	int m_iY;
};
#实现
Coordinate &operator-(Coordinate &c)
{
    c.m_iX = -C.m_iX;
    c.m_iY = -C.m_iY;
    return c;
}
```



## 函数模板

```c++
template <class T>
T max(T a, T b)
{
    return(a > b)? a: b;
}

template <int size>
void display()
{
    cout << size << endl;
}
```



### 多参数模板

```c++
template <typename t, typename C>
void display(T a, C b)
{
    cout <<a <<" " <<b << endl; 
}
```



## 类模板

```c++
//类内定义
template <class T>
    class MyAarry
    {
        public:
        	void display(){}
        private:
			T *m_pArr;
    };
//类外定义，每次定义成员函数都需要在前面添加 template<class T>
template <class T>
    void MyArray<T>::display()
    {}
#实现
int main(void)
{
    MyArray<int>arr;
    arr.display();
    return 0;
}
```



## 标准模板库

### 向量

|                     |           初始化vector对象的方式            |
| :-----------------: | :-----------------------------------------: |
|    vector<T> v1;    | vector保存类型为T的对象。默认构造函数v1为空 |
|  vector<T> v2(v1);  |              v2是v1的一个副本               |
| vector<T> v3(n, i); |            v3包含n个值为i的元素             |
|  vector<T> v4(n);   |        v4包含有值初始化元素的n个副本        |

### 向量常用函数

|                 |                                  |
| :-------------: | :------------------------------: |
|     empty()     |         判断向量是否为空         |
|     begin()     |       返回向量迭代器首元素       |
|      end()      | 返回向量迭代器末元素的下一个元素 |
|     clear()     |             清空向量             |
|     front()     |            第一个数据            |
|     back()      |           最后一个数据           |
|     size()      |       获得向量中数据的大小       |
| push_back(elem) |         将数据插入向量尾         |
|   pop_back()    |         删除向量尾部数据         |

### 向量迭代器使用

```{c++
int main(void)
{
    vector vec;
    vec.push_back("hello");
    vector<string>::iterator citer = vec.begin();
    for(;citer != vec.end(); citer++)
    {
        cout << *citer << endl; //*citer表示当前指向的元素的本身
    }
    return 0;
}
```



## 链表



##映射

```c++
map<int, string> m;
pair<int, string> p1(10, "Shanghai");
pair<int, string> p1(20, "beijing");
m.insert(p1);
m.insert(p2);
cout << m[10] << endl;
cout << m[20] << endl;
```





## 数据结构

###FIFO:先进先出



队列基本代码(右侧为C语言中实现代码)

```c++
class MyQueue
{
    public:
    	MyQueue(int queueCapacity); //InitQueue(&Q) 创建队列
    	virtual ~MyQueue(); //DestroyQueue(&Q) 销毁队列
    	void ClearQueue(); //ClearQueue(&Q) 清空队列
    	bool QueueEmpty(); //QueueEmpty(Q) 判断队列是否为空
    	int QueueLength() const; //QueueLength(Q) 队列长度
    	bool EnQueue(int element); //EnQueue(&Q, element) 新元素入队
    	bool DeQueue(int &element); //DeQueue(&Q, element) 首元素出队
    	void QueueTraverse(); //QueueTraverse(Q, visit()) 遍历队列
    private:
    	int *m_pQueue; //队列数组指针
    	int m_iQueueLen; //队列元素个数
    	int m_iQueueCapacity; //队列数组容量
}
```





栈类基本代码

```c++
#栈类
MyStack((int size); //分配内存初始化栈空间，设定栈容量，栈顶
~MyStack(); //回收栈空间内存
bool stackEmpty(); //判断栈是否为空，为空返回true，非空返回false
bool stackFull(); //判定栈是否已满，为满返回true，不满返回false
void clearStack(); //清空栈
int stackLength(); //已有元素的个数
void push(char elem); //元素入栈，栈顶上升
char pop(char &elem; //元素出栈，栈顶下降)
void stackTraverse(); //遍历栈中所有元素
```



## 线性表

具有n个数据元素的有限序列

### 顺序表（数组）



顺序表基本代码

```c++
BOOL InitList(List **list); //创建线性表
void DestroyList(List *list); //销毁线性表
void ClearList(List *list); //清空线性表
BOOL ListEmpty(List *list); //判断线性表是否为空
int ListLength(List *list); //判断线性表长度
BOOL GetElem(List *list, int i, Elem *e); //获取指定元素
int LocateElem(List *list, Elem *e); //寻找第一个满足e的数据元素的位序
BOOL PriorElem(List *list,Elem *currentElem, Elem *preElem); //获取指定元素的前驱
BOOL NextElem(List *list,Elem *currentElem, Elem *nextElem); //获取指定元素的后继
BOOL ListInsert(List *list, int i, Elem *e); //在第i个位置插入元素
BOOL ListDelete(List *list, int i, Elem *e); //删除第i个位置的元素
void ListTraverse(List *list); //遍历线性表
```



###链表（静态链表、单链表、循环链表、双向链表）	





## 树

### 二叉树

二叉树基本代码(数组表示)

```c++
//关于数组与树的算法转换，父节点下标*2+1表示左子节点，父节点下标*2+2表示右子节点
BOOL CreateTree(Tree **pTree, Node *pRoot); //创建树
void DestoryTree(Tree *pTree); //销毁树
Node *SearchNode(Tree *pTree, int nodeIndex); //根据索引寻找节点
BOOL AddNode(Tree *pTree, int nodeIndex, int direction, node *pNode); //添加节点
BOOL DeleteNode(Tree *pTree, int nodeIndex, Node *pNode); //删除节点
void TreeTraverse(Tree *pTree); //遍历
```



二叉树基本代码(链表实现)

```c++
Tree(); //创建树
~Tree(); //销毁树
Node *SearchNode(int nodeIndex); //搜索节点
bool AddNode(int nodeIndex, int direction, Node *pNode); //添加节点
bool DeleteNode(int nodeIndex, Node *pNode); //删除节点
void PreorderTraversal(); //前序遍历
void InorderTraversal(); //中序遍历
void PostorderTraversal(); //后序遍历
```

​	



## 图

### 无向图

#### 连通图

任意一个顶点都可以直接或间接地连接到其他顶点的图为连通图

#### 完全图

任意一个顶点都可以直接连接其他顶点的图为完全图，其边数为$n\cdot(n-1)/2$

#### 生成树

当完全树由所有顶点，和仅能连接这些顶点的边所组成的图为生成树，其边数为$n-1$



### 邻接表（记录出弧）

顶点的表示方法：顶点索引；出弧链表头指针；顶点数据

弧的表示方法：弧头顶点索引；下一条弧指针；弧数据



###十字链表（有向图）

顶点的表示方法：顶点索引；顶点数据；以该顶点为弧尾的弧节点指针；以该节点为弧头的弧节点指针

弧的表示方法：弧尾顶点索引；弧头顶点索引；弧尾相同的下一条弧的指针；弧头相同的下一条弧的指针；弧的数据



### 邻接多重表（无向图）

顶点的表示方法：顶点索引；连接该顶点的边；顶点数据

边的表示方法：A顶点索引；B顶点索引；与A顶点相连接的下一条边的指针；与B顶点相连接的下一条边的指针；边数据



##最小生成树

### Prim算法



### Kruskal算法

