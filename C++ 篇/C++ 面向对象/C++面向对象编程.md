# C++ 面向对象编程

## 1. 封装

### 1.1 class类

```C++
class 名称{
    属性;
    行为;
};
```

### 1.2 class与struct的区别

struct默认权限为公开，class默认权限为私有。

除此之外，class与struct都能设置权限，区别在默认权限不同：

```C++
class 名称{
private:   //私有权限，类内可以访问，类外不能访问，子类不能访问
protected: //保护权限，类内可以访问，类外不能访问，子类可以访问
public:    //公共权限，类内可以访问，类外可以访问，子类可以访问
};
```

---

## 2. 对象特性

### 2.1 构造函数（对象初始化）和析构函数（对象清理）

1. 如果我们不写构造和析构函数，编译器会提供

2. 编译器提供的构造和析构函数是空实现

#### 2.1.1 构造函数

主要作用在于创建对象时为对象的成员属性赋值，构造函数由编译器自动调用，无需手动调用

**构造函数语法：**`类名(){}`

1. 没有返回值也不写void

2. 函数名与类名相同

3. 可以有参数，可以发生重载

4. 程序创建对象时会自动调用，且只调用一次

```C++
class Person{
public:
    Person(/* 参数 */){
        // 函数内容
    }
};
```

##### 2.1.1.1 构造函数分类

###### 2.1.1.1.1 按参数分类：

1. 无参（默认）构造

2. 有参构造

###### 2.1.1.1.2 按类型分类

1. 普通构造函数

2. 拷贝构造函数

```C++
class Person{
public:
    Person(const Person &p){
        age = p.age; 
        // 将传入对象的所有属性拷贝到当前对象里
    }

    int age;
};
```

##### 2.1.1.2 构造函数调用

1. **括号法**

```C++
Person p1;       // 默认构造函数调用
Person p2(参数); // 有参构造函数调用
Person p3(p2);   // 拷贝构造函数调用
```

2. 显式法

```C++
Person p1;
Person p2 = Person(参数);
Person p3 = Person(p2);
```

3. 隐式转换法

```C++
Person p1;
Person p2 = 参数;
Person p3 = p2;
```

**拷贝构造函数的调用时机：**

1. 使用一个已经创建完毕的对象来初始化一个新对象

2. 值传递的方式给函数参数传值

3. 以值的方式返回局部对象

##### 2.1.1.3 深拷贝与浅拷贝

1. 浅拷贝：简单的复制拷贝

2. 深拷贝：在堆区重新申请空间，进行拷贝

```C++
class Person{
public:
    int age; // 普通成员
    int* height; // 指针成员，数据在堆区
    
    // 构造函数
    Person(int a, int h) : age(a){
        height = new int(h);
    }
    
    // 拷贝构造函数（深拷贝）：在堆区重新申请空间，进行拷贝
    // 如果不写，编译器提供的默认拷贝构造（浅拷贝）会让两个对象的 height 指向同一块堆区内存，
    // 析构时重复释放同一块内存，导致程序崩溃
    Person(const Person &p) : age(p.age){
        height = new int(*p.height);
    }

    // 析构函数
    ~Person(){
        if(height != nullptr){
            delete height;
            height = nullptr;
        }
    }
    
    void show(){
        cout << "年龄：" << age << " 身高：" << *height << endl;
    }
};

int main(){
    Person p1(20, 175);
    Person p2(p1); // 拷贝构造：p1 和 p2 的 height 各自指向独立的堆区空间

    p1.show();
    p2.show();
    
    return 0;
}
```

#### 2.1.2 析构函数

主要作用在于对象销毁前系统自动调用，执行一些清理工作

**析构函数语法：**`~类名(){}`

1. 没有返回值也不写void

2. 函数名与类名相同，在前面加上\~

3. 不可以有参数，不可发生重载

4. 程序在销毁对象前会自动调用，且只调用一次

```C++
class Person{
public:
    ~Person(){
        // 函数内容
    }
};
```

### 2.2 初始化列表

#### 2.2.1 传统方式初始化

```C++
class Person{
public:
    int age;
    char sex;
    int height;
    
    Person(int a, char s, int h){
        age = a;
        sex = s;
        height = h;    
    }
    
    void show(){
        cout << "年龄:" << age << " 性别:" << sex << " 身高:" << height << endl;
    }
};

int main(){
    Person p(20, 'M', 175);
    p.show();
    
    return 0;
}
```

#### 2.2.2 初始化列表

```C++
class Person{
public:
    int age;
    char sex;
    int height;
    
    Person(int a, char s, int h) : age(a), sex(s), height(h){}
    
    void show(){
        cout << "年龄:" << age << " 性别:" << sex << " 身高:" << height << endl;
    }
};

int main(){
    Person p(20, 'M', 175);
    p.show();
    
    return 0;
}
```

### 2.3 类对象作为类成员

**构造**对象时先有其他类的对象，再有本对象（下面**构造**先有Phone，再有Person；**析构**的时候**反过来**）

```C++
class Phone{
public:
    Phone(string PName){
        cout << "Phone构造函数" << endl;
        Name = PName;
    }
    
    string Name;

    ~Phone(){
        cout << "Phone析构函数" << endl;
    }
};

class Person{
public:
    Person(string Name, string pName) : person_Name(Name), phone_Name(pName){
        cout << "Person构造函数" << endl;
    }

    string person_Name;
    Phone phone_Name;

    ~Person(){
        cout << "Person析构函数" << endl;
    }
};
```

### 2.4 静态成员

- 在成员变量和成员函数前加上**关键字static**，称为静态成员

#### 2.4.1 静态成员变量

- 所有对象共享一份数据，不属于某一对象

- 在编译阶段分配内存

- 类内声明，类外初始化

```C++
class Person{
public:
    static string name;
    static string sex;
    static int age;
};

int Person::age = 18;
string Person::name = "Den";
string Person::sex = "男";

void test(){
    Person p;
    cout << "姓名：" << p.name << " 性别：" << p.sex << " 年龄：" << p.age << endl;
    
    p.age = 19;
    cout << "姓名：" << p.name << " 性别：" << p.sex << " 年龄：" << p.age << endl;
}

int main(){
    test();
    return 0;
}
```

1. 通过对象访问 *p.age*

2. 通过类名访问 *Person::age*

**类外不能访问 *private* 的静态成员变量**

#### 2.4.2 静态成员函数

- 所有对象共享一个函数

- **静态成员函数只能访问静态成员变量**

```C++
class Person{
public:
    static void func(){
        cout << "静态成员函数调用" << endl;
    }
};

void test(){
    Person p;
    
    // 通过对象访问
    p.func();
    
    // 通过类名访问
    Person::func();
}

int main(){
    test();
    return 0;
}
```

1. 通过对象访问 *p.func()*

2. 通过类名访问 *Person::func()*

**类外不能访问 *private* 的静态成员函数**

### 2.5 C++对象模型和this指针

#### 2.5.1 成员变量 和 成员函数 分开储存

```C++
class Person{
public:
    int age; // 非静态成员变量 sizeof(p) = 4，属于类对象上
    static int height; // 静态成员变量 sizeof(p) = 4，不属于类对象上
    void func1(){} // 非静态成员函数 sizeof(p) = 4，不属于类对象上
    static void func2(){} // 静态成员函数 sizeof(p) = 4，不属于类对象上
};

int Person::height = 175;
```

#### 2.5.2 this指针

- **this指针**指向被调用函数所属的对象

- **this指针**隐含在每一个**非静态成员函数**内

**this指针的用途：**

1. 当形参和成员变量同名时（名称冲突），用this指针区分

2. 在类的非静态成员函数中返回对象本身：`return *this`

```C++
class Person{
public:
    Person(int age){
        this->age = age;
    }
    
    // Person 后面不加 & 就是 20 岁
    Person& PersonAddAge(Person &p){  
        this->age += p.age;
        return *this; // this 指向调用者对象（p2），*this 就是 p2 本体
    }
    
    int age;
};

void test1(){
    Person p1(18);
    cout << "p1的年龄: " << p1.age << endl;
}

void test2(){
    Person p1(10);
    Person p2(10);
    p2.PersonAddAge(p1);
    cout << "p2的年龄: " << p2.age << endl;
}

void test3(){
    Person p1(10);
    Person p2(10);
    p2.PersonAddAge(p1).PersonAddAge(p1).PersonAddAge(p1);
    cout << "p2的年龄: " << p2.age << endl;
}
```

### 2.6 空指针访问成员函数

```C++
class Person{
public:
    void showClassName(){
        cout << "this is Person class" << endl; // 不会报错
    }
    
    void showPersonAge(){
        // 防止报错
        // if(this == nullptr){
        //     return;
        // }
        
        cout << "age = " << age << endl; // 会报错
    }

    int age;
};

void test1(){
    Person* p = nullptr;
    
    p->showClassName(); // 不报错
    p->showPersonAge(); // 报错
}
```

### 2.7 const修饰成员函数

**常函数：**

- 成员函数后加const成为常函数

- 常函数内不可以修改成员属性

- 常函数成员属性声明时加mutable关键字后，可以修改

**常对象：**

- 声明对象时在对象名前加上const

- 常对象只能调用常函数

```C++
class Person{
public:
    void showPerson() const{
        // this->age = 20; // 报错：常函数中 this 是 const Person* const，指向常量对象，成员不可修改
        this->height = 175; // mutable 成员在常函数中可以修改
    }
    
    int age = 18;
    mutable int height;
};

void test1(){
    Person p;
    p.showPerson();
}

// 常对象
void test2(){
    const Person p{}; // const 对象不能默认初始化（本类没有用户提供的默认构造函数），用 {} 值初始化
    p.height = 180;
    p.showPerson();
}
```

---

## 3. 友元

对于一些私有*private*属性，可以用友元访问。

友元的关键字为*friend*

### 3.1 **友元的三种实现方式**

#### 3.1.1 全局函数做友元

```C++
class Building{
friend void goodGay(Building *building); // 全局函数goodGay做友元可访问私有成员

public:
    Building();

    string sittingRoom;
    
private:
    string bedRoom;
};

Building::Building(){
    sittingRoom = "客厅";
    bedRoom = "卧室";
}

// 全局函数
void goodGay(Building *building){
    cout << "goodGay 正在访问你的" << building->sittingRoom << endl;
    cout << "goodGay 正在访问你的" << building->bedRoom << endl;
}

void test(){
    Building building;
    goodGay(&building);
}
```

#### 3.1.2 类做友元

- 让一个类访问另一个类的私有成员

```C++
class Building{
friend class GoodGay;

public:
    Building();

    string sittingRoom;
    
private:
    string bedRoom;
};

class GoodGay{
public:
    GoodGay();

    void visit();

    Building *building;
};

Building::Building(){
    sittingRoom = "客厅";
    bedRoom = "卧室";
}

GoodGay::GoodGay(){
    building = new Building;
}

void GoodGay::visit(){
    cout << "goodGay 正在访问你的" << building->sittingRoom << endl;
    cout << "goodGay 正在访问你的" << building->bedRoom << endl;
}

void test(){
    GoodGay gay;
    gay.visit();
}
```

#### 3.1.3 成员函数做友元

```C++
class Building; // 类声明

class GoodGay{
public:
    GoodGay();
    
    void visit();
    
    Building *building;
};

class Building{
friend void GoodGay::visit(); // GoodGay的成员函数

public:
    Building();

    string sittingRoom;
    
private:
    string bedRoom;
};

Building::Building(){
    sittingRoom = "客厅";
    bedRoom = "卧室";
}

GoodGay::GoodGay(){
    building = new Building;
}

void GoodGay::visit(){
    cout << "goodGay 正在访问你的" << building->sittingRoom << endl;
    cout << "goodGay 正在访问你的" << building->bedRoom << endl;
}

void test(){
    GoodGay gay;
    gay.visit();
}
```

---

## 4. C++运算符重载

### 4.1 `+`加号运算符重载

- **定义：为自定义类型实现加法运算，让对象支持 \+ 操作符。**

- **注意：全局函数重载时，参数为两个操作数；返回值通常为局部对象（值返回）。**

- **场景：常用于数值计算类（如向量、矩阵、坐标点）的合并或累加。**

```C++
class Person{
public:
    int age;
    int height;
};

// 全局函数重载
Person operator+(Person &p1, Person &p2){
    Person temp;
    temp.age = p1.age + p2.age;
    temp.height = p1.height + p2.height;
    return temp;
}

void test(){
    Person p1;
    p1.age = 18;
    p1.height = 175;
    
    Person p2;
    p2.age = 10;
    p2.height = 150;

    Person p3 = p1 + p2;
    cout << "p3.age = " << p3.age << endl;
    cout << "p3.height = " << p3.height << endl;
}
```

### 4.2 `<<`左移运算符重载

- **定义：用于自定义类型的输出，常与 **`cout`** 配合，实现像内置类型一样的打印效果。**

- **注意：第一个参数必须为 **`ostream&`**（不能是 **`const`**），第二个参数常加 **`const`**；需返回 **`ostream&`** 以支持链式输出。**

- **场景：调试打印、日志记录、显示对象状态信息。**

```C++
class Person{
public:
    int age;
    int height;
};

ostream& operator<<(ostream &os, const Person &p){
    os << "p.age = " << p.age << endl;
    os << "p.height = " << p.height << endl;
    return os;
}

void test(){
    Person p;
    p.age = 18;
    p.height = 175;

    cout << p;
}
```

### 4.3 `++`递增运算符重载

- **定义：为对象提供自增行为，分为前置（**`++i`**）和后置（**`i++`**）两种形式。**

- **注意：前置返回引用（**`*this`**），后置返回临时对象（值返回）并用 **`int`** 占位区分；后置效率略低。**

- **场景：迭代器、计数器、索引类型等需要自增逻辑的对象。**

```C++
class Integer{
public:
    Integer(){
        num = 0;
    }
    
    // 前置递增，返回引用是为了一直对一个对象进行递增
    Integer& operator++(){
        num++;
        return *this;
    }
    
    // 后置递增，int作为占位参数，用于区分前置递增和后置递增
    Integer operator++(int){
        Integer temp = *this;
        num++;
        return temp;
    }

    friend ostream& operator<<(ostream &cout, const Integer &myint);

private:
    int num;
};

ostream& operator<<(ostream &cout, const Integer &myint){
    cout << myint.num;
    return cout;
}

void test1(){
    Integer myint;
    cout << ++myint << endl;
    cout << myint << endl;
}

void test2(){
    Integer myint;
    cout << myint++ << endl;
    cout << myint << endl;
}
```

### 4.4 `=`赋值运算符重载

- **定义：控制对象间的赋值行为，解决浅拷贝带来的内存重复释放或数据不一致问题。**

- **注意：要处理自赋值（**`if (this == &p) return *this;`**）；先释放旧资源再分配新空间；返回 **`*this`** 以支持链式赋值。**

- **场景：类成员包含指针、文件句柄、动态数组等需要深拷贝的资源管理类。**

```C++
class Person{
public:
    Person(int age){
        my_age = new int(age);
    }

    ~Person(){
        if(my_age != nullptr){
            delete my_age;
            my_age = nullptr;
        }
    }

    Person& operator=(const Person& p){
        // 处理自赋值（如 p1 = p1），否则下面的 delete 会释放自己的数据
        if(this == &p){
            return *this;
        }

        // 检查是否有属性在堆区，如果有则先释放
        if(my_age != nullptr){
            delete my_age;
            my_age = nullptr;
        }

        my_age = new int(*p.my_age);
        return *this;
    }
    
    int *my_age;
};

void test(){
    Person p1(10);
    Person p2(20);
    Person p3(30);
   
    p3 = p2 = p1;

    cout << "p1的年龄为：" << *p1.my_age << endl;
    cout << "p2的年龄为：" << *p2.my_age << endl;
    cout << "p3的年龄为：" << *p3.my_age << endl;
}
```

### 4.5 `==`/`!=`关系运算符重载

- **定义：为自定义类型定义相等与不等比较逻辑，使对象能参与条件判断。**

- **注意：**`==`** 与 **`!=`** 通常成对实现，逻辑互为相反；参数可加 **`const`** 以保证不修改对象。**

- **场景：容器查找、去重、排序比较、业务规则匹配（如用户身份验证）。**

```C++
class Person{
public:
    Person(string name, int age) : my_name(name), my_age(age){}

    bool operator==(Person &p){
        return this->my_name == p.my_name && this->my_age == p.my_age;
    }

    bool operator!=(Person &p){
        return this->my_name != p.my_name || this->my_age != p.my_age;
    }

    string my_name;
    int my_age;
};

void test(){
    Person p1("Curry", 30);
    Person p2("Curry", 30);
    Person p3("Durant", 35);

    if(p1 == p2)
        cout << "p1 和 p2 一样" << endl;
    if(p1 != p3)
        cout << "p1 和 p3 不一样" << endl;
}
```

### 4.6 `()`函数调用运算符重载

- **定义：使对象可以像函数一样被调用，也称“仿函数”或“函数对象”。**

- **注意：可带任意参数和返回值；支持匿名对象调用（**`类名()(参数)`**）。**

- **场景：STL算法中的自定义规则（如排序、遍历）、策略模式、简单闭包行为模拟。**

```C++
class MyPrint{
public:
    void operator()(string txt){
        cout << txt << endl;
    }
};

class MyAdd{
public:
    int operator()(int a, int b){
        return a + b;
    }
};

void test(){
    MyPrint myPrint;
    myPrint("Hello World!");

    MyAdd myAdd;
    cout << myAdd(12, 16) << endl;
    cout << MyAdd()(12, 16) << endl; // 匿名函数对象
}
```

---

## 5. 继承

定义一些类时，下级别成员除了拥有上一级的共性，还有自己特性。使用继承，减少重复代码

### 5.1 继承的基本语法

- 继承可以减少重复代码

- **语法：`class 子类: 继承方式 父类`**

- 子类：派生类

- 父类：基类

```C++
class BasePage{
public:
    void header(){
        cout << "首页、公开课、登录、注册...（公共头部）" << endl;
    }
    void footer(){
        cout << "帮助中心、交流合作、站内地图...（公共底部）" << endl;
    }
    void left(){
        cout << "Java、Python、C++...（公共分类列表）" << endl;
    }
};

class Java: public BasePage{
public:
    void content(){
        cout << "Java学科视频" << endl;
    }
};

class Python: public BasePage{
public:
    void content(){
        cout << "Python学科视频" << endl;
    }
};

class CPP: public BasePage{
public:
    void content(){
        cout << "C++学科视频" << endl;
    }
};

void edge(){
    cout << "----------------------------------------" << endl;
}

void test(){
    Java ja;
    ja.header();
    ja.left();
    ja.content();
    ja.footer();
    edge();

    Python py;
    py.header();
    py.left();
    py.content();
    py.footer();
    edge();

    CPP cpp;
    cpp.header();
    cpp.left();
    cpp.content();
    cpp.footer();
    edge();
}
```

### 5.2 继承方式

#### 5.2.1 公共继承

继承方式：`class B: public A`

| 父类成员权限 | 子类中访问权限 |
|---|---|
| public | public |
| protected | protected |
| private | 不可访问 |

#### 5.2.2 保护继承

继承方式：`class B: protected A`

| 父类成员权限 | 子类中访问权限 |
|---|---|
| public | protected |
| protected | protected |
| private | 不可访问 |

#### 5.2.3 私有继承

继承方式：`class B: private A`

| 父类成员权限 | 子类中访问权限 |
|---|---|
| public | private |
| protected | private |
| private | 不可访问 |

**注意：父类的 private 成员同样会被继承，只是不可访问。**

### 5.3 继承中的对象模型

父类中所有**非静态成员**属性都会被子类继承

```C++
class A{
public:
    int a;
protected:
    int b;
private:
    int c;
};

class B: public A{
public:
    int d;
};

void test(){
    cout << "size of class B = " << sizeof(B) << endl; // 16
}
```

### 5.4 继承中的构造和析构顺序

**构造（父）→构造（子）→析构（子）→析构（父）**

```C++
class Father{
public:
    Father(){
        cout << "Father构造函数" << endl;
    }
    ~Father(){
        cout << "Father析构函数" << endl;
    }
};

class Son: public Father{
public:
    Son(){
        cout << "Son构造函数" << endl;
    }
    ~Son(){
        cout << "Son析构函数" << endl;
    }
};

void test(){
    Son s;
}
```

### 5.5 继承同名成员处理方式

- 访问子类同名成员，**直接访问**即可

- 访问父类同名成员，需要加**作用域**

```C++
class Father{
public:
    Father(){
        age = 40;
    }
    
    void func(){
        cout << "Father 的 func 函数调用" << endl;
    }
    
    int age;
};

class Son: public Father{
public:
    Son(){
        age = 14;
    }
    
    void func(){
        cout << "Son 的 func 函数调用" << endl;
    }
    
    int age;
};

void test(){
    Son s;
    cout << "s.age = " << s.age << endl;
    cout << "s.Father::age = " << s.Father::age << endl;
    s.func();
    s.Father::func();
}
```

### 5.6 继承同名静态成员处理方式

静态成员和非静态成员出现同名，处理方式一致

- 访问子类同名成员，**直接访问**即可

- 访问父类同名成员，需要加**作用域**

```C++
class Father{
public:
    static int age;
    static void func();
};
int Father::age = 40;
void Father::func(){
    cout << "Father::func()" << endl;
}

class Son: public Father{
public:
    static int age;
    static void func();
};
int Son::age = 14;
void Son::func(){
    cout << "Son::func()" << endl;
}

void test(){
    Son s;
    cout << "s.age = " << s.age << endl;
    cout << "s.Son::age = " << Son::age << endl;
    cout << "s.Father::age = " << s.Father::age << endl;
    cout << "s.Son::Father::age = " << Son::Father::age << endl;
    s.func();
    Son::func();
    s.Father::func();
    Son::Father::func();
}
```

### 5.7 多继承语法

C\+\+中允许一个类继承多个类

语法：`class 子类: 继承方式 父类1, 继承方式 父类2...`

**多继承可能引发重名，需加作用域。因此在C\+\+开发中不推荐使用多继承**

```C++
class Father1{
public:
    Father1(){
        age = 40;
    }
    
    int age;
};

class Father2{
public:
    Father2(){
        age = 50;
    }
    
    int age;
};

class Son: public Father1, public Father2{
public:
    Son(){
        age = 16;
    }
    
    int age;
};

void test(){
    Son s;
    cout << "size of Son = " << sizeof(s) << endl; // 12
    cout << "Father1's age = " << s.Father1::age << endl;
    cout << "Father2's age = " << s.Father2::age << endl;
    cout << "Son's age = " << s.age << endl;
}
```

### 5.8 菱形继承（钻石继承）

- 概念：两个派生类继承同一个基类，又有某个类同时继承这两个派生类。

- 问题：1\. 最后的子类可能继承两份一样的数据（来自公共基类）→**虚继承**；2\. 使用数据可能产生二义性→**加作用域**

```C++
class Animal{
public:
    int age;
};

class Cat: virtual public Animal{ };

class Dog: virtual public Animal{ };

class Cat_Dog: public Cat, public Dog{ };

void test(){
    Cat_Dog cd;
    cd.Cat::age = 6;
    cd.Dog::age = 8;
    cout << "cd.Cat::age = " << cd.Cat::age << endl; // 8
    cout << "cd.Dog::age = " << cd.Dog::age << endl; // 8
}
```

---

## 6. 多态

### 6.1 多态基本概念

多态分为两类：

- 静态多态：**函数重载**和**运算符重载**属于静态多态，复用函数名

- 动态多态：**派生类**和**虚函数**实现运行时多态

两者区别：

- 静态多态的函数地址**早绑定**（**编译阶段**）

- 动态多态的函数地址**晚绑定**（**运行阶段**）

***❗动态多态要求***

1. ***有继承***

2. ***子类重写父类虚函数***

3. ***父类指针或引用指向子类对象***

```C++
class Animal{
public:
    void voice(){
        cout << "动物" << endl;
    }
};

class Cat: public Animal{
public:
    void voice(){
        cout << "喵" << endl;
    }
};

void test1(Animal &animal){
    animal.voice(); // 动物，早绑定问题
}

void test2(){
    Cat c;
    test1(c); // 父类引用指向子类对象，输出"动物"（早绑定）

    cout << "size of Animal: " << sizeof(Animal) << endl; // 1，类似于空对象
}
```

**解决方式↓，加个**`virtual`**

```C++
class Animal{
public:
    virtual void voice(){
        cout << "动物" << endl;
    }
};

class Cat: public Animal{
public:
    void voice(){
        cout << "喵" << endl;
    }
};

void test1(Animal &animal){
    animal.voice(); // 喵
}

void test2(){
    Cat c;
    test1(c); // 父类引用指向子类对象，输出"喵"（晚绑定）

    cout << "size of Animal: " << sizeof(Animal) << endl; // 8，虚函数指针占8（Win64）/4（Win32）字节
}
```

虚函数（表）指针`vfptr`，占4字节（32位）/8字节（64位）

### 6.2 纯虚函数和抽象类

有了纯虚函数的类是抽象类

- 纯虚函数语法：`virtual 返回值类型 函数名(参数) = 0;`

**抽象类特点：**

1. 无法实例化对象

2. 子类必须重写抽象类中的纯虚函数，否则也属于抽象类

```C++
class Base{
public:
    virtual void func() = 0;

    virtual ~Base() {} // 虚析构：基类指针 delete 派生类对象时才能正确调用派生类析构函数
};

class Son: public Base{
public:
    virtual void func(){
        cout << "func调用" << endl;
    }
};

void test(){
    Base *b = new Son();
    b->func();
    delete b;
}
```

### 6.3 虚析构和纯虚析构

多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码

- 解决方式：将**父类的析构函数**改为**虚析构**或者**纯虚析构**

```C++
class Animal{
public:
    Animal(){
        cout << "Animal构造函数调用" << endl;
    }

    // 虚析构解决内存泄露
    virtual ~Animal(){
        cout << "Animal析构函数调用" << endl;
    }

    virtual void voice() = 0;
};

class Cat: public Animal{
public:
    Cat(string name){
        cout << "Cat构造函数调用" << endl;
        m_Name = new string(name);
    }

    virtual void voice(){
        cout << *m_Name << "：喵" << endl;
    }

    ~Cat(){
        cout << "Cat析构函数调用" << endl; // 不调用
        if(m_Name != nullptr){
            delete m_Name;
            m_Name = nullptr;
        }
    }

    string *m_Name;
};

void test(){
    Animal *animal = new Cat("狸花");
    animal->voice();
    // 父类指针在析构的时候，不会调用子类的析构函数，导致出现内存泄漏
    delete animal;
}
```

**纯虚析构写法：**`virtual ~Animal() = 0;`，且必须在类外提供定义：`Animal::~Animal(){}`

---

## 7. 文件操作

C\+\+中对文件操作需要包含`<fstream>`

- 文件类型分为2种：

1. **文本文件**：以**ASCII码**形式储存在计算机中

2. **二进制文件**：以**二进制**形式储存在计算机中，一般读不懂

- 操作文件三大类

1. `ofstream`：写文件

2. `ifstream`：读文件

3. `fstream`：读写文件

### 7.1 文本文件

#### 7.1.1 **写文件**

```C++
#include <fstream> // 1.包含头文件

ofstream ofs; // 2.创建流对象

ofs.open("文件路径", 打开方式); // 3.打开文件

ofs << "写入的内容..."; // 4.写文件

ofs.close(); // 5.关闭文件
```

|**打开方式**|**解释**|
|---|---|
|**ios::in**|**为读文件而打开文件**|
|**ios::out**|**为写文件而打开文件**|
|**ios::ate**|**初始位置：文件尾**|
|**ios::app**|**追加方式写文件**|
|**ios::trunc**|**如果文件存在先删除，再创建**|
|**ios::binary**|**二进制方式**|

#### 7.1.2 读文件

```C++
#include <fstream> // 1.包含头文件

ifstream ifs; // 2.创建流对象

ifs.open("文件路径", 打开方式); // 3.打开文件

if(ifs.is_open()){            // 4.是否打开成功
    cout << "文件打开成功" << endl;
}else{
    cout << "文件打开失败" << endl;
    return 1;
}

string str;             // 5.读文件
while(getline(ifs, str))
    cout << str << endl;

ifs.close(); // 6.关闭文件
```


