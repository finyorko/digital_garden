---
{"dg-publish":true,"permalink":"/01/c/","title":"[[C++笔记整理]]","tags":[null]}
---

# C++笔记

### 常用数据结构


```cpp
// vector
vector<int> nums={1,2,3};
vector<int> nums={};
vector<int> nums(n,m); // 长度是 n，默认值是 m,如果不给默认值，则默认值是0
vector<int> a(nums.begin(), nums.end()); // 用nums初始化vector
vector<vector<int>> dp(n,vector<int>(m,0)); 一个 n 行 m 列二维数组，默认值是0
nums.insert(nums.begin()+1,3);//索引1处插入元素3
nums.insert(ans.end(), count, x); // 在最后加入count个x
nums.push_back(3);// 最后插入元素3
nums.pop_back();//弹出最后一个
nums.erase(num.begin() + 1); // 删除第二个元素
nums.erase(vec.begin(), vec.begin() + 3 );//删除第1个元素，到第4个元素
nums[1]=3;//索引1元素修改为3
if(find(nums.begin(),nums.end(),target)!=nums.end())cout<<"find";
for(auto c:nums){cout<<c;}//遍历
nums.size();//是无符号数，改成有符号(int)nums.size()
nums.empty();
sort(nums.begin(),nums.end()); // 在原数组排序，无返回值

// deque
deque<string> q;

q.front();	//返回队列头部元素
q.back();	//返回队列尾部元素，
q.push_front("Hello ");		//将"Hello "入队首
q.push_back("world");		//将"world"入队尾
q.pop_front();			//将队列头部的元素删除，无返回值
q.pop_back();			//将队列尾部的元素删除，无返回值
q.empty();	//返回bool值，表示队列是否为空，此时不为空，返回0
q.size();	//返回队列的大小

// queue
queue<int>q;
q.pop(); //似乎没有返回值，只是删除第一个进来的元素，要确保里面有元素，否则会报错
q.back();// 确保有元素
q.front();// 确保有元素
q.empty();
q.push(1); //添加元素1

// priority_queue
//对于基础类型 默认是大顶堆
priority_queue<int> a; 
//等同于 priority_queue<int, vector<int>, less<int>> a;
priority_queue<int, vector<int>, greater<int> > c;  //这样就是小顶堆
priority_queue<string> c;// 大顶堆 ///cbd abcd abc

// deque
deque<int>d;//定义
d.push_back(1);//尾追加
d.push_back(2);
d.push_front(0);//放到最前面
for(auto x:d){cout<<x;}
d.pop_front();
d.pop_back();
d.front();
d.back();

// stack
stack<int>stack1;
//加入元素
stack1.push(1);
//删除元素
stack1.pop();
//取栈顶元素
stack1.top();
//获取大小
stack1.size();

// string
//定义字符串
string str1="abc";  //必须用双引号
str1.length();//获取字符串长度
string str2="bc";
str1+str2;//字符串之间可以相加,不可以减法
char c1='a';//必须用单引号
//取子字符串
sub1=str1.substr(pos,length);//pos表示位置索引，length表示子字符串长度
// 增加，只能是字符
str1.push_back('a');
//获取字符串最后一个元素
str1.back();
//遍历
string s1="abc";
for(auto c:s1){
    cout<<c<<" ";
}

// set
set<int> res;
res.insert(1)//插入元素
res.erase(1)//删除元素1
if(res.find(1)!=res.end())//查找元素

// unordered_map
// 定义一个哈希map
unordered_map<char ,int > hash;
unordered_map<int, vector<int>>::iterator it;
//添加元素
hash['c']=1;
hash.insert({'b',2});
//清空
hash.clear();
// 删除元素
hash.erase('c');//删除某个元素，传入参数key
if( hash.find(key)!=hash.end()) cout<<"找到"<<hash[key];
```

### acm输入输出

#### 整型数组输入

在终端的一行中输入**固定数目**的整型数字，并存到数组中，中间以**空格**分隔。

```cpp
/*
3
1 2 3
*/
int n;
cin >> n;
vector<int> nums(n);
for (int i = 0; i < n; ++i){
	cin >> nums[i];
}
```

---

在终端的一行中输入**非固定数目**的整型数字，并存到数组中，中间以**空格（或者其他单字符,./）**分隔。

```cpp
/*
1 2 3
*/
//代码通过cin.get()从缓存中读取一个字节，这样就扩充了cin只能用空格和TAB两个作为分隔符。
//这很精巧。发现是’\n’就知道一行结束了 
vector<int> nums;
int num;
while(cin>>num){
	nums.push_back(num);
    if(getchar() == '\n')
        break;
}
```

---

在终端的一行中输入**固定数目**的整型数字，并存到数组中，中间以**（其他单字符,./）**分隔。

```cpp
/*
3
1,2,3
*/
//代码通过cin.get()从缓存中读取一个字节，这样就扩充了cin只能用空格和TAB两个作为分隔符。
//这很精巧。发现是’\n’就知道一行结束了 
int m;
cin >>  m;
char sep;
vector<int> nums(m);
// 输入前m-1个
for (int i = 0; i < m - 1; ++i){
	cin >> nums[i] >> sep;
}
// 输入最后一个，因为最后一个后面没有分割字符
cin >> nums[m - 1];
```

#### 字符串输入

给定**一行字符串**，每个字符串用**空格**间隔，**一个样例为一行**

```cpp
/*
daa ma yello
*/
int main()
{
    string str;
    vector<string> strs;
    while (cin >> str) {
        strs.push_back(str);
        if (getchar() == '\n') {  //控制测试样例
            break;
        }
    }
    return 0;
}
```

给定**一行字符串**，每个字符串用**逗号**间隔，**一个样例为一行**

```cpp
/*
adsf,asdfasd,sdfaf
*/
// 使用getline 读取一整行字符串到字符串input中，然后使用字符串流stringstream，读取单个数字或者字符。每个字符中间用','间隔
int main() {
    vector<string> strs;
    string input;
    getline(cin, input); //读取一行
    string str;
    stringstream ss(input);
    while(getline(ss, str,',')){
        strs.push_back(str);
    }
    return 0;
}
```

给定**一行字符串**，每个字符串用**空格**间隔，**一个样例为一行**

```cpp
/*
12 43 44
*/
int main() {
	string input;
    vector<int> nums;
	while (getline(cin, input)) {  //读取一行
		stringstream data(input);  //使用字符串流
		int num = 0, sum = 0;
		while (data >> num) {
			nums.push_back(num);
		}
	}
	return 0;
}
```

#### 字符串输入到数组

输入n行字符串，逗号分开，存到数组里

```cpp
/*
3
1,2,3,3,2
4,5,6,1,3
7,8,9,3,4
*/
#include<bits/stdc++.h>
using namespace std;

int main()
{
    int n;
    cin >> n;
    vector<vector<int>> vec(n,vector<int>(5));
    for (int i = 0; i < n;++i){//注意这里一定要有i控制，用while容易一直输入导致错误
        string s;
        cin >> s;
        replace(s.begin(), s.end(), ',', ' ');
        istringstream input(s);
        string tmp;
        for (int j = 0; j < 5;++j){//内层循环也很重要
            input >> tmp;
            vec[i][j] = stoi(tmp);
        }
    }

    for (int i = 0; i < vec.size(); i++){
    	for (int j = 0; j < vec[0].size(); j++){
    		cout << "输出是" << vec[i][j] << endl;
    	}
    }
    return 0;
}

// 方式2
int main() {
    int n;
    cin >> n;
    cin.ignore(); //忽略换行符
    vector<vector<int>> nums(n, vector<int>());
    string input;
    for(int i=0;i<n;i++){
        getline(cin, input); //读取一行
        string str;
        stringstream ss(input);
        while(getline(ss, str,',')){
            nums[i].push_back(stoi(str));
        }
    }
    return 0;
}
```



## C++计控课件

### 动态内存分配，new返回的是指针

```cpp
使用动态内存分配实现指针访问数组
int **a;
a = new int*[3];/*a为指针，指向具有3个”int*”
数据的数组(即元素均为指针的数组)*/
for(i=0;i<3;i++)
	*(a+i) = new int[4];
/*分配3批动态空间(每批为4个int大小)，并使上述数
组元素指针指向它们。此时的a实际上是一个3行4列的数
组，也可通过a[i][j]的形式访问各元素。*/
```

### 类

#### static

- 类的静态**数据**成员为该类的所有对象所共享，在类定义中对静态数据成员进行说明。必须在类外文件作用域中的某个地方对静态数据成员赋初值（且要按如下的格式）：
  <类型> <类名>::<静态数据成员> = <初值>;
- 类的静态**函数**成员没有this指针，从而无法处理不同调用者对象的各自数据成员值。通常情况下，类的静态函数只处理类的静态数据成员。

#### const

- 类的**常量数据成员**必须进行初始化，而且只能通过构造函数的成员初始化列表的方式来进行；在对象被创建以后，其常量数据成员的值就不允许被修改（只可读取其值，但不可进行改变）
- **常量函数成员**只有权读取相应对象的内容，但无权修改它们。

#### 友元函数

友元函数不是类的成员函数，在函数体中访问对象的成员，必须用对象名加运算符“.”加对象成员名。但友元函数可以访问类中的所有成员，一般函数只能访问类中的公有成员。

友元函数不受类中的访问权限关键字限制，可以把它放在类的公有、私有、保护部分，但结果一样。

某类的友元函数的作用域并非该类域。如果该友元函数是另一类的成员函数，则其作用域为另一类的类域，否则与一般函数相同。

##### 将普通函数说明为某类的友元函数

该函数定义在类外，说明格式为： friend <返回值类型> <函数名>( <参数表> ); 

【例如】在类A的定义中，将函数Func说明为类A的友元函数：`friend void Func();`

##### 将其它类的成员函数说明为某类的友元函数

前提是已有其它类的定义，说明格式为： friend <返回值类型> <类名>::<函数名>(<参数 表>); 

【例如】在类A的定义中，将类B的成员函数说明为 类A的友元函数：`friend void B::Func(); `

#### 友元类

将一个类B说明为另一个类A的友元类，类B中的所有函数都是类A的友元函数，可以访问类A中的所有成员。

说明方式： friend <类名>; 

```cpp

class A{
    ……
    friend B;
    ……
};
```

友元类的关系是单向的，如果说明类B是类A的友元类，不等于类A也是类B的友元类

友元类的关系不能传递，如果类B是类A的友元类，而类C是类B的友元类，不等于类C是类A的友元类

**除非确有必要，一般不把整个类说明为友元类，而把成员函数说明为友元函数**

友元的概念破坏了类的封装性，但有助于数据共享，能够提高程序的效率

例：

```cpp
#include<iostream.h>
#include<math.h>
class pixel{
	int x,y;
public:
    pixel(int x0, int y0){
        x=x0;
        y=y0;
    }
    void printxy(){
    	cout<<"pixel: ("<<x<<","<<y<<")"<<endl;
    }
    friend double getLen(pixel p1, pixel p2);
    //友元函数（原型）
};

double getLen(pixel p1, pixel p2){//友元函数在类体外定义
double xd=double(p1.x-p2.x);//使用pixel类的私有成员x
double yd=double(p1.y-p2.y);
return sqrt(xd*xd+yd*yd);
}
void main() {
    pixel p1(0,0), p2(10,10);
    p1.printxy();
    p2.printxy();
    cout<<"getLen(p1,p2)="<<getLen(p1,p2)<<endl;
}
// 程序执行后的显示结果如下：
// pixel: (0,0)
// pixel: (10,10)
// getLen(p1,p2)=14.1421
```

#### 类的嵌套（不建议使用）

一个类的说明包含在另一个类说明中，即为类的嵌套

```cpp
class CC{
    class C1{…};
    public:
    class C2{…};
    C1 f1(C2); …
};
```

在main主函数中：
C1 a,b;
C2 a,b;
CC::C1 a,b;
• 以上都是非法的
CC::C2 a,b;
• 是合法的，它说明了两个嵌套类C2的对象。嵌套类的使用并不方便，不宜多用

#### 类中的运算符重载

允许使用如下两种不同方式来定义运算符重载函数：

- 以类的友元函数方式定义：**所有运算分量**必须显式地列在本友元函数的**参数表**中，而且这些参数类型中至少要有一个应该是**说明该友元的类类型**或是**对该类的引用**
- 以类的公有成员函数方式定义：总以当前调用者对象(*this)作为该成员函数的隐式第一运算分量，若所定义的运算多于一个运算对象时，才将其余运算对象显式地列在该成员函数的参数表中

**被用户重载的运算符，其优先级、结合性、以及运算分量个数都必须与系统中的本原运算符相一致。**

不可重载的5个运算符：`.  ::  ?:  .*  sizeof`

只能以类成员而不能以友元身份重载的运算符：`() = []  ->  type`

【例】自定义一个称为point的类，其对象表示平面上的一个点(x,y)，并通过友员方式对该类重载二目运算符“==”、“+”、“-”、“^”和一目运算符“-”，用来判断两个对象是否相等，求出两个对象的和、差、距离，以及将组成一个对象的数值取反。以下使用**友元函数**实现

```cpp
#include <iostream>
#include <math.h>
using namespace std;
class point {
    double x,y;
    public:
    point(double x0=0, double y0=0);
    friend point operator-(point pt);//重载函数
    friend point operator+(point pt1, point pt2);
    friend point operator-(point pt1, point pt2);
    friend bool operator==(point pt1, point pt2);
    friend double operator^(point pt1, point pt2);
    void display();
};
point::point (double x0, double y0) {
	x=x0; y=y0;
}
point operator - (point pt) {
    point temp;
    temp.x=-pt.x;
    temp.y=-pt.y;
    return temp;
}
point operator + (point pt1, point pt2) {
    point temp;
    temp.x=pt1.x+pt2.x;
    temp.y=pt1.y+pt2.y;
    return temp;
}
point operator - (point pt1, point pt2) {
    // 自行实现
	cout<<""<<endl;
    return pt1;
}
bool operator == (point pt1, point pt2) {
    //自行实现
	return true;
}
double operator ^ (point pt1, point pt2) {
    // 自行实现
    return 1.2;
}
void point::display () {
	cout <<"( "<<x<<", "<<y<<" )"<<endl;
}
int main() {
    point s0, s1(1.2, -3.5),s2(-1, 2.8),s3(6, 6);
    point s4, s5;
    cout<<"s0="; s0.display();
    cout<<"s1="; s1.display();
    cout<<"s2="; s2.display();
    cout<<"s3="; s3.display();
    s0=s1+s2; //将调用“operator +”函数
    cout<<"s0=s1+s2="; s0.display();
    s4=s1-s2;
    cout<<"s4=s1-s2="; s4.display();
    s5=-s1;
    cout<<"s5=-s1="; s5.display();
    if(s1==s1) cout<<"s1==s1"<<endl;
    cout<<"s2^s3="<<(s2^s3)<<endl;
    return 0;
}
```

#### 拷贝构造函数

1. 一种特殊的构造函数，具有一般构造函数的特性

2. 只含有一个形参，且为本类对象的引用

3. 拷贝构造函数的原型为： <类名>(<类名>&);

4. 作用是使用一个已存在的对象去初始化另一个正在创建的对象

5. 类对象间的赋值，由拷贝构造函数实现（通过深拷贝或浅拷贝的方式）

6. 系统会自动生成缺省的拷贝构造函数，实现对象间的拷贝（浅拷贝）

   

当程序出现以下**三类情况**时，自动调用拷贝构造函数：
{ #35dfeb}


- 使用下列形式的说明语句
  - <类名> <对象名2>(<对象名1>); 
  - <类名> <对象名2> = <对象名1>; 
- 对象作为函数的赋值参数
- 函数的返回值为类的对象

##### 显式拷贝构造函数（深拷贝）

在某些情况下，必须在类定义中给出显式拷贝构造函数，以实现用户指定的拷贝功能（深拷贝）

- 假设在某类的普通构造函数中分配并使用了某些系统资源，而且在该类的析构函数中释放了这些资源。 如果执行“浅拷贝”，使两个对象使用相同的系统资源，调用析构函数将会两次释放相同的资源而导致错误。（见“默认拷贝构造函数产生的问题”） 
- 给出显式的拷贝构造函数，可以在实现拷贝的过程 中，为“被拷贝”的对象分配新的系统资源，避免了重复释放资源的错误

##### 默认拷贝构造函数产生的问题

该方式两个对象均使用了同一个name，导致析构函数报错。

```cpp
//MyClass.h
class MyClass
{
	char *name;
public:
	MyClass(char *);
    ~MyClass();
        void print();
};

//MyClass.cpp
#include<iostream.h>
#include<string.h>
#include"MyClass.h"
MyClass::MyClass(char *n){
    name = new char[strlen(n)+1];
    strcpy(name,n);
    }
MyClass::~MyClass(){
	delete []name;
}
void MyClass::print(){
	cout<<"Member name="<<name<<endl;
}

//Main.cpp
#include<iostream.h>
#include"MyClass.h"
void main(){
    char *p = new char[20];
    cin>>p;
    MyClass MyObj(p);
    MyClass MyObj1=MyObj; //执行拷贝构造函数
    MyObj.print();
    MyObj1.print();
}
```

需要做的改动：

```cpp
class MyClass
{
    char *name;
public:
    MyClass(char *);
    MyClass(MyClass&);
    ~MyClass();
    void print();
};
//自定义拷贝构造函数，实现深拷贝
MyClass::MyClass(MyClass& CopyObj){
    name = new char[strlen(CopyObj.name)+1];
    strcpy(name,CopyObj.name);
}
```

##### 自定义拷贝构造函数无法解决“[[01 学习笔记/C++笔记整理#^35dfeb\|三类情况]]”之外的对象赋值问题

两种解决方案：

- 重载赋值运算符“=”
- 自定义成员函数，实现对象赋值

```cpp
//MyClass.h
class MyClass
{
	char *name;
public:
    MyClass(char *);
    MyClass(MyClass&);
    
    //1. 重载赋值运算符
    MyClass operator=(MyClass&);
    //2. 自定义成员函数，实现对象赋值
    void CopyMyClassObj(MyClass);
    
    ~MyClass();
    void print();
};

//MyClass.cpp
#include<iostream>
#include <string.h>
using namespace std;
MyClass::MyClass(char *n)
{
    name = new char[strlen(n)+1];
    strcpy(name,n);
}
MyClass::MyClass(MyClass& CopyObj)
{
    name = new char[strlen(CopyObj.name)+1];
    strcpy(name,CopyObj.name);
}
void MyClass::CopyMyClassObj(MyClass temp){
    name =new char[strlen(temp.name)+1];
    strcpy(name,temp.name);
}
MyClass MyClass::operator=(MyClass &temp){
    name = new char[strlen(temp.name)+1];
    strcpy(name,temp.name);
    return *this;
}
MyClass::~MyClass(){
    delete []name;
}
void MyClass::print(){
	cout<<"Member name="<<name<<endl;
}

//Main.cpp
#include<iostream>
using namespace std;
int main(){
    char *p = new char[20];
    char test[] = "lmnop";
    cin>>p;
    MyClass MyObj(p); //执行构造函数
    MyClass MyObjSwap(test);	//执行构造函数
    MyClass MyObj1 = MyObj;		//执行拷贝构造函数
    MyObj = MyObjSwap;	//未执行拷贝构造函数，执行赋值运算符重载函数
    MyObjSwap = MyObj1;	//未执行拷贝构造函数，执行赋值运算符重载函数
    //MyObj.CopyMyClassObj(MyObjSwap);//调用成员函数
    //MyObjSwap.CopyMyClassObj(MyObj1);//调用成员函数
    MyObj.print();
    MyObj1.print();
    MyObjSwap.print();
    return 0;
}
```

#### 类的继承与派生

派生类可以访问基类的保护成员（在基类中以关键字protected标识的成员）

- 第二步中，新成员如是成员函数，参数表也必须一样，否则是重载。

- 第三步中，独有的新成员才是继承与派生的核心特征。
- 第四步是重写构造函数与析构函数，派生类不继承这两种函数。
- 不管原来的函数是否可用一律重写可免出错。

<img src="https://amorcode.oss-cn-hangzhou.aliyuncs.com/amor_img/class1.png" alt="image-20240318203649496" style="zoom:50%;" />

#### 派生类

##### 派生类的访问权限

<img src="https://amorcode.oss-cn-hangzhou.aliyuncs.com/amor_img/class2.png" alt="image-20240318203951016" style="zoom:50%;" />

基类的私有成员不可在派生类中被存取

- public派生方式：使基类的公有成员和保护成员在派生类中仍然是公有成员和保护成员，而基类的私有成员不可在派生类中被存取。 
- protected派生方式：使基类的公有成员和保护成员在派生类中都变为保护成员，而基类的私有成员不可在派生类中被存取。 
- private派生方式：使基类的公有成员和保护成员在派生类中都变为私有成员，而基类的私有成员不可在派生类中被存取。

派生类的成员可根据访问权限分为四类

1. 不可访问的成员：基类的private私有成员被继承过来后，这些成员在派生类中是不可访问的。
2. 私有成员：包括在派生类中新增加的private私有成员以及从基类私有继承过来的某些成员。这些成员在派生类中是可以访问的。
3. 保护成员：包括在派生类中新增加的protected保护成员以及从基类继承过来的某些成员。这些成员在派生类中是可以访问的。
4. 公有成员：包括在派生类中新增加的public公有成员以及从基类公有继承过来的基类的public成员。这些成员不仅在派生类中可以访问，而且在建立派生类对象的模块中，也可以通过对象来访问它们

##### 派生类构造函数

派生类构造函数执行的一般次序如下：

1. 调用各基类的构造函数，调用顺序为**派生继承时的基类声明顺序**。
2. 若派生类含有对象成员的话，调用各对象成员的构造函数，调用顺序按照派生类中**对象成员的声明顺序**。 
3. 执行派生类构造函数的函数体。

```cpp
class CD:public CB,public CC{
	int d;
public:
	//符合上面第1点，构造顺序：先CB,后CC
	CD(int n1,int n2,int n3,int n4):CC(n3,n4),CB(n2){
		d=n1; cout<<"CD::d="<<d<<endl;
	}
~CD(){
	cout<<"CDobj is destructing"<<endl;
	}
};
```

加大难度：

```cpp
class CD:public CB,public CC {
    int d;
    CC obcc; // CC对象
    CB obcb; // CB对象
public:
	CD(int n1,int n2,int n3,int n4):CC(n3,n4), CB(n2), obcb(100+n2),obcc(100+n3,100+n4) {
		d=n1; cout<<"CD::d="<<d<<endl;
	};
	~CD(){
		cout<<"CDobj is	destructing"<<endl;
    };
};
//先基类CB、CC，再对象成员CC、CB，最后派生类CD
```

##### 友元的继承

- 基类的友元不继承：如果基类有友元类或友元函数，则其派生类不因继承关系也有此友元类或友元函数。
- **如果基类是某类的友元，则这种友元关系是被继承的**。即，被派生类继承过来的成员，如果原来是某类的友元，那么它作为派生类的成员仍然是某类的友元。

总之：基类的友元不一定是派生类的友元；基类的成员是某类的友元，则其作为派生类继承的成员仍是某类的友元。

(你可以用我爸的东西，但是不能用我的东西；我爸可以用你的东西，我就可以用你的东西)

##### 静态成员的继承

- 如果基类中被派生类继承的成员是静态成员，则其静态属性也随静态成员被继承。
- 如果基类的静态成员是公有的或是保护的，则它们被其派生类继承为派生类的静态成员。即：这些成员通常用“<类名>::<成员名>”方式引 用或调用。这些成员无论有多少个对象被创建，都只有一个拷贝。它为基类和派生类的所有对象所共享。

##### 赋值兼容性问题

派生类对象间的赋值操作依据下面的原则：

- 如果派生类有自己的赋值运算符的重载定义，即按重载后的运算符含义处理。
- 派生类未定义自己的赋值操作，而基类定义了赋值操作，则系统自动定义派生类赋值操作，其中基类成员的赋值按基类的赋值操作进行。 
- 二者都未定义专门的赋值操作，系统自动定义缺省赋值操作（按位进行拷贝）。

基类对象和派生类对象之间允许有下述的赋值关系（一句话概括：**允许将派生类对象“当作”基类对象来使用**）： 

- 基类对象=派生类对象

  只赋“共性成员”部分，反方向的赋值【派生类对=基类对象】不被允许 

- 指向基类对象的指针=派生类对象的地址

  下述赋值不允许：指向派生类类型的指针=基类对象的地址。注：访问非基类成员部分时，要经过指针类型的强制转换

- 基类的引用=派生类对象

  下述赋值不允许：派生类的引用=基类对象。通过引用只可以访问基类成员部

##### 二义性问题

###### 单继承时父类与子类间重名成员的处理

单继承时父类与子类间成员重名时，按如下规定进行处理：对子类而言，不加类名限定时默认为是处理子类成员，而要访问父类重名成员时，则要通过类名限定。

```cpp
class CB {
public:
	int a;
	CB(int x){a=x;}
    void showa(){
    	cout<<"Class CB -- a="<<a<<endl;
	}
};
class CD:public CB {
public:
    int a; //与基类a同名
    CD(int x, int y):CB(x){a=y;}
    void showa(){ //与基类showa同名
    	cout<<"Class CD -- a="<<a<<endl;
	}
	void print2a() {
        cout<<"a="<<a<<endl; //子类a
        cout<<"CB::a="<<CB::a<<endl; //父类a
	}
};
// 执行
int main()
{
    CB CBobj(12);
    CBobj.showa();
    CD CDobj(48, 999);
    CDobj.showa();      // 子类的showa	999
    CDobj.CB::showa();  // 父类的showa	48
    CBobj.showa();		// 12
    // 将子类对象赋值给基类对象
    CBobj = CDobj;		
    CBobj.showa();		// 48
    return 0;
}
/*
程序执行后的显示结果如下：
Class CB -- a=12 
Class CD -- a=999
Class CB -- a=48 
Class CB -- a=12 
Class CB -- a=48 
*/
```

###### 多继承情况下二基类间重名成员的处理

多继承情况下二基类间成员重名时，按如下方式进行处理：对子类而言，不加类名限定时默认为是处理子类成员，而要访问父类重名成员时，则要通过类名限定。

```cpp
#include <iostream>

class CB1 {
public:
    int a;
    CB1(int x) { a = x; }
    void showa() {
        std::cout << "Class CB1 ==> a=" << a << std::endl;
    }
};

class CB2 {
public:
    int a;
    CB2(int x) { a = x; }
    void showa() {
        std::cout << "Class CB2 ==> a=" << a << std::endl;
    }
};

class CD : public CB1, public CB2 {
public:
    int a; // 与二基类数据成员a同名
    CD(int x, int y, int z) : CB1(x), CB2(y) {
        a = z;
    }
};

int main() {
    CB1 CB1obj(11);
    CB1obj.showa();
    
    CD CDobj(101, 202, 909);
    CDobj.showa();          // 子类showa
    CDobj.CB1::showa();     // 父类showa

    std::cout << "CDobj.a=" << CDobj.a << std::endl;
    std::cout << "CDobj.CB2::a=" << CDobj.CB2::a << std::endl;

    return 0;
}
/* 输出如下：
Class CB1 ==> a=11
Class CB2 ==> a=909
Class CB1 ==> a=11
CDobj.a=909
CDobj.CB2::a=202
*/
```

###### 多级混合继承(非虚拟继承)包含两个基类实例情况的处理

多级混合继承情况下，若类D从两条不同“路径”同时对类A进行了一般性继承（非虚拟继承）的话，则类D的对象中会同时包含着两个类A的实例。此时，对类D而言，要通过类名限定来指定访问两个类A实例中的哪一个。

- class A
- class B : public A
- class C : public A
- class D : public B, public C

结构：( ((A) B ) ( (A) C ) D ) 

举例：类A--人员类； 类B--学生类； 类C--助教类； 类D--学生助教类

#### 虚基类

以virtual说明基类继承方式，例`class B1:virtual public B`

多级混合继承情况下，若类D从两条不同“路径”同时对类A进行了虚拟继承的话，则类D的对象中只包含着类A的一个实例，这种继承也称为共享继承。被虚拟继承的基类A被称为虚基类（注意，虚基类的说明是在**定义派生类时**靠增加关键字virtual来指出的）。

说明格式：`class <派生类名> : virtual <派生方式> <基类名> { <派生类体> };`

- class A
- class B : virtual public A
- class C : virtual public A
- class D : public B, public C

结构：( ( (A) B C ) D )

- 主要用来解决多继承时可能发生的对同一基类继承多次而产生的二义性问题。系统进行“干预”，**在派生类中只生成公共基类A的一个拷贝**，从而可用于解决二义性问题
- 为最远的派生类提供唯一的基类成员，而不重复产生多次复制

```
#include <iostream>

class A {
public:
    int a;
    void showa() { std::cout << "a=" << a << std::endl; }
};

class B : virtual public A {
public:
    int b;
};

class C : virtual public A {
public:
    int c;
};

class D : public B, public C {
public:
    int d;
};

int main() {
    D Dobj; // 说明D类对象
    
    // 若非虚拟继承时会出错!
    // -- 因为“D::a”具有二义性
    Dobj.a = 11;
    
    Dobj.b = 22;
    
    // 若非虚拟继承时会出错!
    // -- 因为“D::showa”具有二义性
    Dobj.showa();

    std::cout << "Dobj.b=" << Dobj.b << std::endl;
    return 0;
}
```

#### 类的多态性与虚函数

多态性：函数重载、静态联编、函数超载、动态联编、纯虚函数、抽象基类、虚函数

##### 函数重载(overloading)

函数重载（overloading）指的是，允许多个不同函数使用同一个函数名，但要求这些同名函数具有不同的参数表。

- 参数表中的参数个数不同; 
- 参数表中对应的参数类型不同; 
- 参数表中不同类型参数的次序不同

系统对函数重载这种多态性的分辨与处理，是在编译阶段完成的 -- **静态联编(static binding)**

##### 函数超载(overriding):覆盖（重写）

- 仅在基类与其派生类的范围内实现；

- 允许多个不同函数使用完全相同的函数名、函数参数表以及函数返回类型；

函数覆盖是**动态**多态的一种实现方式，它在运行时根据对象的实际类型确定调用哪个函数。

覆盖（重写）的规则：

（1）函数名相同；

（2）参数相同；

（3）基类函数必须有virtual关键字；

##### 隐藏（重定义）

隐藏（重定义）：隐藏是指派生类的函数屏蔽了与其同名的基类函数。

（1）如果派生类的函数与基类的函数同名，但是参数不同，此时，**不论有无virtual关键字**，基类的函数将被隐藏（注意别与重载混淆）；

（2）如果派生类的函数与基类的函数同名，并且参数也相同，但是基类函数没有virtual关键字。此时，基类的函数被隐藏（注意别与覆盖混淆）；

规则：

1. 在不同的作用于中（分别在基类和派生类）
2. 函数名相同
3. 在基类和派生类中，只要不构成重写，就是重定义

##### 虚函数(virtual function)

- 在定义某一基类(或其派生类)时，若将其中的某一函数成员的属性说明为virtual，则称该函数为虚函数(virtual function)
- 虚函数的使用与函数超载（覆盖）密切相关。若基类中某函数被说明为虚函数，则意味着其派生类中也要用到与该函数同名、同参数表、同返回类型、但函数(实现)体不同的这同一个所谓的超载函数。 
- 在基类中定义虚函数，其派生类的同原型函数默认为虚函数，可省略virtual关键字。**只在派生类中定义虚函数没有意义**

##### 动态联编(dynamic binding)

- 与虚函数以及程序中使用指向基类的指针密切相关。
- C++规定，基类指针可以指向其派生类的对象（也即，可将派生类对象的地址赋给其基类指针变量），但反过来不可以。这一点正是函数超载及虚函数用法的基础。

【例】假设`inte_algo`为基类，其中说明了一个虚函数`integrate`（用来计算定积分），并在其三个派生类中，也说明了该虚函数`integrate`，可使用函数`integrateFunc`来实现调用不同虚函数integrate的目的。注意：**调用的将是不同派生类的integrate函数**。

**基类对象不可调用，只有指针可以。**

**基类对象不可调用，只有指针可以。**

**基类对象不可调用，只有指针可以。**

```cpp
void integrateFunc(inte_algo *p){
	//基类指针p可指向任一派生类的对象
	p->integrate(); //调用的将是不同派生类的integrate函数
}
// 主调函数处使用：
integrateFunc( <某派生类的类对象地址> ); 
// 根据不同的派生类对象，调用各自派生类的成员函数
// 在编译阶段，系统无法确定究竟要调用哪一个派生类的integrate。
// 此种情况下，将采用动态联编方式来处理：
// 在运行阶段，通过p指针的当前值，去动态地确定对象所属类，而后找到对应虚函数。
```

##### 纯虚函数

如果不准备在基类的虚函数中做任何事情，则可使用如下的格式将该虚函数说明成纯虚函数： 

```cpp
virtual <函数原型> = 0; 
```

纯虚函数不能被直接调用，它只为其派生类的各虚函数规定了一个一致的“原型规格”，该虚函数的实现将在它的派生类中给出。

含有纯虚函数的基类称为抽象基类。 

- 不可使用抽象基类来说明并创建它自己的对象，只有在创建其派生类对象时，才有抽象基类自身的实例伴随而生。
- 抽象基类是其各派生类之共同点的一个抽象综合，通过它，再“加上”各派生类的特有成员以及对基类中那一纯虚函数的具体实现，方可构成一个具体的实用类型。
- 如果一个抽象基类的派生类中没有定义基类中的那一纯虚函数、而只是继承了基类之纯虚函数的话，则这个派生类还是一个抽象基类（其中仍包含着继承而来的那一个纯虚函数)

##### 综合实例

```cpp
#include <iostream>
using namespace std;

class Vehicle //交通工具，基类
{
public:
    Vehicle(int w) {
        weight = w;
    }
    virtual void ShowMe() {
        cout << "我是交通工具！重量为" << weight << "吨" << endl;
    }
protected:
    int weight;
};

class Car: public Vehicle //汽车，派生类
{
public:
    Car(int w, int a): Vehicle(w) {
        aird = a;
    }
    virtual void ShowMe() override {
        cout << "我是汽车！排气量为" << aird << "CC" << endl;
    }
protected:
    int aird;
};

class Boat: public Vehicle //船，派生类
{
public:
    Boat(int w, float t): Vehicle(w) {
        tonnage = t;
    }
    virtual void ShowMe() override {
        cout << "我是船！排水量为" << tonnage << "顿" << endl;
    }
protected:
    float tonnage;
};

int main() {
    Vehicle *pv = new Vehicle(10);
    pv->ShowMe(); // 输出：我是交通工具！重量为10吨

    Car car(15, 200);
    Boat boat(20, 1.25f);
    
    pv = &car;
    pv->ShowMe(); // 基类指针，实现动态联编，输出：我是汽车！排气量为200CC

    Vehicle v(10); // 创建一个基类对象
    v = boat; // 将派生类对象赋值给基类对象
    v.ShowMe(); // 基类对象访问虚函数，未实现动态联编，输出：我是交通工具！重量为20吨

    pv = &boat;
    pv->ShowMe(); // 基类指针，实现动态联编，输出：我是船！排水量为1.25顿

    return 0;
}
/* 输出
我是交通工具！重量为10吨
我是汽车！排气量为200CC
我是交通工具！重量为20吨
我是船！排水量为1.25顿
*/
```

若上面改成抽象基类，Vehicle中：`virtual void ShowMe() = 0;`

```cpp
int main() {
    Vehicle *pv = new Vehicle(10)//ERROR
    pv ->ShowMe();//ERROR 纯虚函数不能直接调用
    Car c(15,200);
    Boat b(20,1.25f);
    Vehicle * pv = &c;//用派生类对象进行初始化
    pv->ShowMe();//基类指针，实现动态联编，输出：我是汽车！排气量为200CC
    pv = &b;
    pv -> ShowMe();//基类指针，实现动态联编，输出：我是船！排水量为1.25顿
    return 0;
}
/* 输出
我是汽车！排气量为200CC
我是船！排水量为1.25顿
*/
```

### 模板

#### 函数模板

函数模板定义的一般格式为： 

​	`template < class 类型形参名1，... ， class 类型形参名n > 返回类型 函数模板名(形参表) {函数体 }`

注意: 

- 应在“返回类型”或“形参表”或“函数体”中使用上述的“类型形参名” 。
- 调用处则类似于一般函数，用户只需给出具体的实参。
- 模板函数调用时，不进行实参到形参类型的自动转换。
- 从物理意义上，函数模板类似于重载

```cpp
#include <iostream>

template <class T>
T max(T a, T b) {
    if (a > b)
        return a;
    else
        return b;
}

int main() {
    int i1 = -11, i2 = 0;
    double d1, d2;
    
    std::cout << max(i1, i2) << std::endl;
    std::cout << max(23, -56) << std::endl;
    std::cout << max('f', 'k') << std::endl;
    
    std::cin >> d1 >> d2;
    std::cout << max(d1, d2) << std::endl;
    
    //出错! 不进行实参到形参类型的自动转换
    std::cout<<"max(23,-5.6) = "<<max(23,-5.6)<<endl;
    
    return 0;
}

```

定义一个函数模板与一个函数，它们都叫做min，C++允许这种函数模板与函数同名的所谓重载使用方法。但注意，在这种情况下，每当遇见函数调用时，C++编译器都将首先检查是否存在重载函数，若匹配成功则调用该函数，否则再去匹配函数模板。



#### 类模板

类模板（带**类型参数**或**普通参数**的类）用来定义具有共性的一组类

- “共性”通过类模板参数体现
- 通过类模板的定义，类中的某些数据成员、某些成员函数的参数、某些成员函数的返回值都可以是任意类型的
- 可将程序所处理的对象（数据）的类型参数化，从而使同一段程序可用于处理多种不同类型的对象（数据）

##### 类模板的定义方式

`template <类型形参或普通形参1的说明,...,类型形参或普通形参n的说明> class 类模板名 { 带上述**类型形参**或**普通形参**名的类定义体 };`

- 类型形参 ：class 类型形参名
- 普通形参 ：数据类型 普通形参名 
- 类模板名 ：标识符

##### 类模板的说明

类定义体中应使用上述的“类型形参名”及 “普通形参名”。

- 利用类模板说明类对象时，要随类模板名同时给出对应于类型形参或普通形参的具体实参（从而实例化为一个具体的类）。说明格式为： 类模板名 < 形参1的相应实参，... ，形参n的相应实 参 > 
- 注意：类型形参的相应实参为**类型名**，而普通形参的相应实参必须为一个**常量**。

##### 类模板的成员函数

类模板的成员函数既可以在类体内进行说明 （自动按内联函数处理），也可以在类体外进行说明。

在类体外说明（定义）时使用如下格式： `template < 形参1的说明，... ，形参n的说明 > 返回类型 类模板名 < 形参1的名字，... ，形参n的名 字 >::函数名( 形参表 ) { ... //函数体 };`

上述的“形参1的名字”来自于“形参1的说明”，由“甩掉”说明部分的“类型”而得，是对类型形参或普通形参的使用。而 “类模板名 < 形参1的名字，... ，形参n的名字 >::”所起的作用正是在类体外定义成员函数时在函数名前所加的类限定符!

##### 类模板的实例化

- 不能使用类模板来直接生成对象：类型参数是不确定的

- 必须先为模板参数指定“实参”：即为模板“实例化”
- 实例化格式 `类模板名 <具体的实参表>`
- 利用类模板生成对象 `类模板名 <具体的实参表> 对象名称`

【例如】对具有一个类型参数T的类模板 TestClass，在类体外定义其成员函数getData时的大致样式如下：

```cpp
template <class T>
T TestClass<T>::getData( 形参表 ) {
... //函数体
};
// 其中的“TestClass<T>::”所起的作用正是在类体外定义成员函数时在函数名前所加的类限定符! 
```

###### 仅使用类型参数的类模板示例

```cpp
#include <iostream>
using namespace std;

template <class T>
class TestClass {
public:
    T buffer[10];
    T getData(int j);
};

template <class T>
T TestClass<T>::getData(int j) {
    return *(buffer + j);
}

int main() {
    TestClass<char> ClassInstA;
    char cArr[6] = "abcde";
    for (int i = 0; i < 5; i++)
        ClassInstA.buffer[i] = cArr[i];
    for (int i = 0; i < 5; i++) {
        char res = ClassInstA.getData(i);
        cout << res << " ";
    }
    cout << endl;

    TestClass<double> ClassInstF;
    double fArr[6] = {12.1, 23.2, 34.3, 45.4, 56.5, 67.6};
    for (int i = 0; i < 6; i++)
        ClassInstF.buffer[i] = fArr[i] - 10;
    for (int i = 0; i < 6; i++) {
        double res = ClassInstF.getData(i);
        cout << res << " ";
    }
    cout << endl;
}
/* 输出
a b c d e
2.1 13.2 24.3 35.4 46.5 57.6
*/
```

###### 仅使用普通参数(非类型参数)的类模板示例

```cpp
#include <iostream>
using namespace std;

template <int i>
class TestClass {
public:
    int buffer[i];
    int getData(int j);
};

template <int i>
int TestClass<i>::getData(int j) {
    return *(buffer + j);
}

int main() {
    TestClass<6> ClassInstF;
    double fArr[6] = {12.1, 23.2, 34.3, 45.4, 56.5, 67.6};
    for (int i = 0; i < 6; i++)
        ClassInstF.buffer[i] = fArr[i] - 10;
    for (int i = 0; i < 6; i++) {
        double res = ClassInstF.getData(i);
        cout << res << " ";
    }
    cout << endl;
}
/* 输出
2 13 24 35 46 57
*/
```

###### 既使用类型参数又使用普通参数的类模板示例

```cpp
#include <iostream>
#include <cstring> // Include for strcpy
using namespace std;

template <class T, int i>
class TestClass {
public:
    T buffer[i];
    T getData(int j);
};

template <class T, int i>
T TestClass<T, i>::getData(int j) {
    return *(buffer + j);
}

int main() {
    TestClass<char, 5> ClassInstA;
    char cArr[6] = "abcde";
    strcpy(ClassInstA.buffer, cArr);
    for (int i = 0; i < 5; i++) {
        char res = ClassInstA.getData(i);
        cout << res << " ";
    }
    cout << endl;

    TestClass<double, 6> ClassInstF;
    double fArr[6] = {12.1, 23.2, 34.3, 45.4, 56.5, 67.6};
    for (int i = 0; i < 6; i++)
        ClassInstF.buffer[i] = fArr[i] - 10;
    for (int i = 0; i < 6; i++) {
        double res = ClassInstF.getData(i);
        cout << res << " ";
    }
    cout << endl;

    return 0;
}
/*
输出
a b c d e
2.1 13.2 24.3 35.4 46.5 57.6
*/
```



#### 类模板若干问题的说明

##### 类模板的静态成员

人话：一个实例化**类**，共享一个静态成员

类模板也允许有静态成员。实际上，它们是类模板之实例化类的静态成员。也就是说，对于一个类模板的每一个实例化类，其所有的对象共享其静态成员。

```cpp
template <class T> class C{
static T t； //类模板的静态成员t
};
```

类模板的静态成员在模板定义时是不会被创建的， 其创建是在类的实例化之后

```
CA<int>aiobj1, aiobj2；
CA<char>acobj1, acobj2；
// 对象aiobj1和aiobj2将共享实例化类CA<int>的静态成员 int t ，
// 而对象acobj1，acobj2将共享实例化类CA<char>的静态成员 char t。
```

##### 类模板的友元

类模板定义中允许包含友元。讨论类模板中的友元函数，因为说明一个友元类，实际上相当于说明该类的成员函数都是友元函数。 

- 该友元函数为**一般函数**，则它将是该类模板的所有实例化类的友元函数。
- 该友元函数为**一函数模板**，但其类型参数与类模板的类型参数无关。则该函数模板的**所有**实例化（函数）都是类模板的所有实例化类的友元。
- 更复杂的情形是，该友元函数为**一函数模板**， 且它**与类模板的类型参数有关**。例如，函数模板可以用该类模板作为其函数参数的类型。在友元函数模板定义与相应类模板的类型参数有关时，该友元函数模板的实例有可能只是该类模板的**某些特定实例化**（而不是所有实例化）类的友元

##### 类型参数检测与特例版本

大多数类模板不能任意进行实例化。也就是说类模板的类型参数往往在实例化时不允许用任意的类（类型）作为“实参”。模板的“实参”不当，主要会在实例化后的函数成员调用中体现出来。

```cpp
template <class T> 
class stack {
    //栈中元素类型为T 的stack 类模板
    T num[MAX]; //num 中存放栈的实际数据
    int top; //top 为栈顶位置
public:
    stack () { top=0; } //构造函数
    void push (T a) { num[top++]=a; } //将数据a“压入”栈顶
    void showtop(){ // 显示栈顶数据
        //模板中通用的showtop，显示栈顶的那一个T 类型的数据
        //（必须为可直接通过运算符“<<”来显示的数据）
        if (top==0) cout << "stack is empty!"<< endl;
        else cout<<"Top_Member:"<<num[top-1]<<endl;
    }
};
```

在类模板 `stack` 中，以下实例化都是可行的：
- `stack<int> i1, i2;`
- `stack<char> c1, c2;`
- `stack<float> f1, f2;`

但如果采用用户定义类型而又未在该类中对运算符 `<<` 进行重载时，就会产生问题，例如：
```cpp
stack<complex> com1, com2;
```

由于在执行 `com1.showtop()` 函数时，将需要对 `complex` 类型的数据 `num[top-1]` 通过使用运算符 `<<` 来进行输出，而系统和用户都没有定义过这种操作，因此，类模板 `stack` 的实例化 `stack<complex>` 就是不可行的了。

如果用户在上述情况下，需要使 `stack<complex>` 可行，有几个办法：

- 对于类 `complex` 追加插入运算符 `<<` 的重载定义；
- 也可在类模板 `stack` 的定义中增加一个“特例版本”（也称“特殊版本”）的定义。例如在上例中，可以在类模板定义之后给出如下形式的特例版本：

```cpp
void stack<complex>::showtop() {
    /* 专用于 complex 类型的 showtop（专门补充的“特例版本”），
       显示栈顶的那一个 complex 型数据。其中的 stack<complex> 为一个实例化后的模板类 */
    if (top == 0)
        cout << "stack is empty!" << endl;
    else
        cout << "Top_Member:" << num[top - 1].get_r() << ", " << num[top - 1].get_i() << endl;
}
```

- 假设自定义的复数类型 `complex` 中具有公有的成员函数 `get_r()` 和 `get_i()`，用于获取复数的实部和虚部。在实例化 `stack<complex>` 时，将按照特例版本的定义进行处理。

- 概括地说，当处理某一类模板中的可变类型 `T` 型数据时，如果处理算法并不能对所有的 `T` 类型取值做统一的处理，此时可通过使用专门补充的所谓特例版本来对具有特殊性的那些 `T` 类型取值做特殊处理。
- 也可以对函数模板或类模板的个别函数成员补充其“特例版本”定义。
- 例如，可将该例的 `showtop` 功能进一步划分，让 `showtop` 调用另一个新增加的 `show` 函数，而由 `show` 函数具体考虑对两种情况的处理：一种处理可直接通过运算符“<<”来显示的数据，另一种“特例版本”专用于处理 `complex` 类型的数据。

```cpp
class complex { //复数类型complex
    double real, image;
public:
    //...
};

template <class T>
class stack {
    T data[20];
    int top;
public:
    void showtop(void); //显示栈顶数据
    //...
}; 

template <class T>
void stack<T>::showtop(void)
//通用的showtop
//T类型数据，可直接通过“<<”来一次性输出的数据
{
    if (top == 0)
        cout << "stack is empty!" << endl;
    else
        cout << "Top_Member:" << data[top - 1] << endl;
}

void stack<complex>::showtop(void)
//专用于complex类型的showtop
//显示栈顶的那一个complex型数据，它不可直接通过
//“<<”一次性输出!
{
    if (top == 0)
        cout << "stack is empty!" << endl;
    else
        cout << "Top_Member:" << data[top - 1].get_r() << "," << data[top - 1].get_i() << endl;
} 

void main() {
    stack<int> s1;
    for (int i = 1; i <= 6; i++)
        s1.push(2 * i);
    //“压入”：2,4,6,8,10,12 (栈顶为12)
    s1.showtop(); //调用模板中通用的showtop
    stack<complex> s1c;
    complex c1(1.1, 1.111), c2(2.2, 2.222);
    s1c.push(c1);
    s1c.push(c2);//“压入”复数c2 (处于栈顶)
    s1c.showtop();
    //调用专门补充的“特例函数”showtop
}
```

##### 按照不同的方法派生类模板

通过继承可以产生派生类。通过继承同样可产生派生的类模板

- 一般类（其中不使用类型参数的类）作基类，派生出类模板（其中要使用类型参数）

  ```cpp
  class CB {
      // CB 为一般类（其中不使用类型参数），它将作为类模板 CA 的基类
      // ...
  };
  
  template <class T> 
  class CA: public CB {
      // 被派生出的 CA 为类模板，使用了类型参数 T，其基类 CB 为一般类
      T t; // 私有数据为 T 类型的
  public:
      // ...
  };
  ```

- 类模板作基类，派生出新的类模板。但**仅基类中用到类型参数T**，而派生的类模板中不使用T

  ```cpp
  template <class T> 
  class CB {
      // CB 为类模板（其中使用了类型参数 T），它将作为类模板 CA 的基类
      T t; // 私有数据为 T 类型的
  public:
      T gett() { return t; } // 用到类型参数 T
      // ...
  };
  
  template <class T> 
  class CA : public CB<T> {
      // CA 为类模板，其基类 CB 也为类模板。注意，类型参数 T 将被“传递”给基类 CB，本派生类中并不使用该类型参数 T
      double t1; // 私有数据成员
  public:
      // ...
  };
  /*基类的名字应为实例化后的“CB<T>”而并非仅使用“CB”。例如，在本例的派生类说明中，要对基类进行指定时必须使用“CB<T>”而不可只使用“CB”*/
  ```

- 类模板作基类，派生出新的类模板，且基类与派生类中均使用**同一个类型参数T**。

  ```cpp 
  template <class T> 
  class CB {
      // CB 为类模板（其中使用了类型参数 T），它将作为类模板 CA 的基类
      T t; // 数据成员为 T 类型的
  public:
      T gett() { // 用到类型参数 T
          return t;
      }
      // ...
  };
  
  template <class T> 
  class CA : public CB<T> {
      /* CA 为类模板，其基类 CB 也为类模板。注意，类型参数 T 将被“传递”给基类 CB；
         本派生类中也将使用这同一个类型参数 T */
      T t1; // 数据为 T 类型的
  public: 
      // ...
  }; 
  ```

- 类模板作基类，派生出新的类模板，但基类中使用**类型参数T2**，而派生类中使用另一个**类型参数T1**（而不使用T2）。

  ```cpp
  template <class T2>
  class CB {
      // CB 为类模板（其中使用了类型参数T2），它将作为类模板CA的基类
      T2 t2; // 数据为T2 类型的
  public:
      // ...
  };
  
  template <class T1, class T2>
  class CA : public CB<T2> {
      /* CA为类模板，其基类CB也为类模板。注意，类型参数T2将被“传递”给基类CB；
      本派生类中还将使用另一个类型参数T1 */
      T1 t1; // 数据为T1类型的
  public:
      // ...
  };
  
  ```

  

#### 综合实例

## C++Primer笔记

### C++多态

多态

- 静多态
  - 函数重载
  - 泛型编程
- 动多态
  - 虚函数（覆盖）

#### 静多态

静多态是在编译的时候就确定调用函数的类型，函数重载就是一个典型的静多态。

函数重载的规则：

（1）函数名相同；

（2）参数的个数不同、参数的类型不同、参数的顺序不同，均可构成重载；

（3）**返回类型不同不可以构成重载**；

（4）相同的范围（在同一个类中）。

#### 动多态

在运行的时候才确定调用哪个函数，动态绑定。实质是子类重新定义了父类的虚函数，父类的指针根据赋给它的不同子类的指针，动态的调用属于子类的该函数，这样的函数调用在编译期间无法确定。

##### 覆盖

覆盖是指派生类中重新定义父类中的函数，其函数名、参数列表、返回类型必须同父类中的相对应被覆盖的函数严格一致，只有函数体不同。

覆盖的规则：

（1）函数名相同；

（2）参数相同；

（3）基类函数必须有virtual关键字；

---

位运算知识：https://leetcode.cn/problems/power-of-two/solutions/658587/5chong-jie-fa-ni-ying-gai-bei-xia-de-wei-6x9m/

`cout << ::used;` 将输出全局作用域中 `used` 变量的值。

---  -   - -

引用必须被初始化

```cpp
int val = 1024;
int &refVal1 = ival;	// refVal1指向val
int &refVal2; 			// 报错：引用必须被初始化
```

-----

生成空指针的方法：（三者等价）

```cpp
int *p1 = nullptr;	// 等价于 int *p1 = 0;
int *p2 = 0;		// 直接将p2初始化为字面常量0
int *p3 = NULL;		
```

---

对于两个类型相同的合法指针，可以用相等操作符(==)或不相等操作符(!=)来比较它们，比较的结果是布尔类型。如果两个指针存放的地址值相同，则它们相等;反之它们不相等。这里两个指针存放的地址值相同(两个指针相等)有三种可能:

- 它们都为空
- 都指向同一个对象
- 都指向了同一个对象的下一地址。

需要注意的是，一个指针指向某对象，同时另一个指针指向另外对象的下一地址，此时也有可能出现这两个指针值相同的情况，即指针相等。

---

如果想在多个文件之间共享const对象，必须在变量的定义之前添加extern关键字。

```cpp
// file_1.cc定义并初始化了一个常量，该常量能被其他文件访问
extern const int bufSize = fcn();
// file_1.h 头文件
extern const int bufSize; // 与file_1.cc中定义的bufSize是同一个
```

---

```cpp
int const *p; // 指针常量，常量的指针。指针指向一个常量
int *const p; // 常量指针，指针是个常量，必须初始化。
```

---

##### 24、C++中const和static的作用

**static**

- 不考虑类的情况
  - 隐藏。所有不加static的全局变量和函数具有全局可见性，可以在其他文件中使用，加了之后只能在该文件所在的编译模块中使用
  - 默认初始化为0，包括未初始化的全局静态变量与局部静态变量，都存在全局未初始化区
  - 静态变量在函数内定义，始终存在，且只进行一次初始化，具有记忆性，其作用范围与局部变量相同，函数退出后仍然存在，但不能使用
- 考虑类的情况
  - static成员变量：只与类关联，不与类的对象关联。定义时要分配空间，不能在类声明中初始化，必须在类定义体外部初始化，初始化时不需要标示为static；可以被非static成员函数任意访问。
  - static成员函数：不具有this指针，无法访问类对象的非static成员变量和非static成员函数；**不能被声明为const、虚函数和volatile**；可以被非static成员函数任意访问

**const**

- 不考虑类的情况
  - const常量在定义时必须初始化，之后无法更改
  - const形参可以接收const和非const类型的实参，例如// i 可以是 int 型或者 const int 型void fun(const int& i){ //...}
- 考虑类的情况
  - const成员变量：不能在类定义外部初始化，只能通过构造函数初始化列表进行初始化，并且必须有构造函数；不同类对其const数据成员的值可以不同，所以不能在类中声明时初始化
  - const成员函数：const对象不可以调用非const成员函数；非const对象都可以调用；不可以改变非mutable（用该关键字声明的变量可以在const成员函数中被修改）数据的值

补充一点const相关：const修饰变量是也与static有一样的隐藏作用。只能在该文件中使用，其他文件不可以引用声明使用。 因此在头文件中声明const变量是没问题的，因为即使被多个文件包含，链接性都是内部的，不会出现符号冲突。


## 面试题目整理
##### 在不使用额外空间的情况下，交换两个数
```cpp
// 方法一
	x = x + y;
    y = x - y;
    x = x - y;
// 方法二 异或
	x = x^y;// 只能对int,char..
    y = x^y;
    x = x^y;
```

#####  

##### 
##### 
##### 
##### 
##### 






