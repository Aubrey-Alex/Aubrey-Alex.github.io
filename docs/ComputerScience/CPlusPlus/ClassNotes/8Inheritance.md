# 继承

## 代码复用方式

### 组合（Composition）

组合是将现有对象构建成新对象的方式，表示"has-a"关系。

!!! quote "Alan Kay观点"
    每个对象都有自己的内存，由其他对象组成。

### 继承（Inheritance）

继承是通过克隆现有类，然后对克隆进行添加和修改来实现的。

??? note "继承的定义"
    继承是将一个类的行为或实现定义为另一个类的超集的能力。

## 继承的作用

- 语言实现技术
- 面向对象设计方法论的重要组成部分
- 允许共享设计
  - 成员数据
  - 成员函数
  - 接口
- C++的关键技术

## DoME 案例研究

!!! example "多媒体娱乐数据库"
    DoME（Database of Multimedia Entertainment）是一个允许存储CD和DVD信息的应用程序。
    
    我们可以：

    - 输入CD和DVD的信息
    - 搜索特定条件（如特定艺术家的所有CD或特定导演的所有DVD）

### CD和DVD类的属性

=== "CD类"
    - 专辑标题
    - 艺术家（乐队或歌手名称）
    - CD上的曲目数
    - 总播放时间
    - 'got it'标志（表示我是否拥有这张CD的副本）
    - 评论（任意文本）

=== "DVD类"
    - DVD标题
    - 导演名称
    - 播放时间（定义为主要内容的播放时间）
    - 'got it'标志（表示我是否拥有这张DVD的副本）
    - 评论（任意文本）

### 初始设计：类图

```
+-------------+        +---------------+
|  Database   |        |               |
+-------------+   +--->|      CD       |
| vector<CD>  |   |    +---------------+
| vector<DVD> |   |    | title         |
+-------------+   |    | artist        |
| addCD()     |---+    | tracks        |
| addDVD()    |---+    | playingTime   |
| list()      |   |    | gotIt         |
+-------------+   |    | comment       |
                  |    +---------------+
                  |    | print()       |
                  |    +---------------+
                  |    
                  |    +---------------+
                  |    |               |
                  +--->|      DVD      |
                       +---------------+
                       | title         |
                       | director      |
                       | playingTime   |
                       | gotIt         |
                       | comment       |
                       +---------------+
                       | print()       |
                       +---------------+
```

### 数据库实现代码

```cpp
class Database {
    vector<CD> cds;
    vector<DVD> dvds;
public:
    void addCD(CD &aCD);
    void addDVD(DVD &aDVD);
    void list() {
        for (auto x:cds) { cd.print();}
        for (auto x:dvds) { dvd.print();}
    }
};
```

### 初始设计的问题

!!! danger "代码重复问题"
    - CD和DVD类非常相似（大部分代码相同）
    - 使维护困难/工作量大
    - 可能因不正确的维护而引入bug
    - Database类中也有代码重复

??? question "思考问题"
    - CD和DVD类非常相似，代码大部分相同，只有少数差异
    - Database类中所有操作都做了两次 - 一次用于CD，一次用于DVD
    - 如果添加新的媒体类型会怎样？

## 使用继承改进设计

!!! success "解决方案"
    继承允许我们将一个类定义为另一个类的扩展。

### 改进后的类图

```
             +---------------+
             |     Item      |
             +---------------+
             | title         |
             | playingTime   |
             | gotIt         |
             | comment       |
             +---------------+
             | print()       |
             +---------------+
                   ∧
                   |
        +----------+----------+
        |                     |
+---------------+     +---------------+
|      CD       |     |      DVD      |
+---------------+     +---------------+
| artist        |     | director      |
| tracks        |     +---------------+
+---------------+     | print()       |
| print()       |     +---------------+
+---------------+
```

### 使用继承

- 定义一个超类：Item
- 为CD和DVD定义子类
- 超类定义共同属性
- 子类继承超类属性
- 子类添加自己的属性

### 继承层次结构

```
             +---------------+
             |     Item      |
             +---------------+
                   ∧
                   |
        +----------+----------+
        |                     |
+---------------+     +---------------+
|      CD       |     |      DVD      |
+---------------+     +---------------+
                            ∧
                            |
                    +---------------+
                    |  VideoGame    |
                    +---------------+
```

### 改进后的数据库实现

```cpp
public void addItem(Item theItem) {
    items.add(theItem);
}

/**
 * Print a list of all currently stored items to the text terminal.
 */
public void list() {
    for(auto item : items) {
        item.print();
    }
}
```

### 继承的优点

- 避免代码重复
- 代码重用
- 更容易维护
- 可扩展性

## 继承的基本概念

### 继承关系

继承表示"Is-A"关系

!!! example ""
    - CD **是一个** Item
    - DVD **是一个** Item

### 继承了什么

- (private) 成员变量
- public 成员函数
- private 成员函数
- protected 成员
- static 成员

### 私有成员变量

- 派生类对象内部包含基类对象，其中包含所有成员变量
- 但派生类**没有**直接访问这些变量的权限
- 必须通过基类的成员函数访问
- 如果派生类有同名变量，它是一个独立的新变量

### 派生类对象和基类转换

!!! note "继承对象的内存结构"
    派生对象包含多个部分：一个包含派生类自身定义的（非静态）成员的子对象，以及对应于派生类继承的每个基类的子对象。

    ```cpp
    class A...
    class B:public A...
    ```

### 公有成员函数

- 它们是派生类的公有成员函数
- 它们定义了类的接口

!!! quote "Alan Kay观点"
    特定类的所有对象都可以接收相同的消息。

### 私有成员函数

- 它们在派生类中**不可访问**

### 受保护成员

- 它们在派生类中完全可访问

### 静态成员

- 它们仍然是类范围的成员

## 继承示例：员工管理

### Employee类定义

```cpp
class Employee {
public:
    Employee( const std::string& name, const std::string& ssn );
    const std::string& get_name() const;
    void print(std::ostream& out) const;
    void print(std::ostream& out, const std::string& msg) const;
protected:
    std::string m_name;
    std::string m_ssn;
};
```

### Employee构造函数

```cpp
Employee::Employee( const string& name, const string& ssn )
    : m_name(name), m_ssn( ssn) {
    // initializer list sets up the values!
}
```

### Employee成员函数

```cpp
inline const std::string& Employee::get_name() const {
    return m_name;
}

inline void Employee::print( std::ostream& out ) const {
    out << m_name << endl;
    out << m_ssn << endl;
}

inline void Employee::print(std::ostream& out, const std::string& msg) const {
    out << msg << endl;
    print(out);
}
```

### Manager类定义

```cpp
class Manager : public Employee {
public:
    Manager(const std::string& name, const std::string& ssn, const std::string& title);
    const std::string title_name() const;
    const std::string& get_title() const;
    void print(std::ostream& out) const;
private:
    std::string m_title;
};
```

## 构造函数和继承

### 继承和构造函数

- 将继承的特征视为嵌入对象
- 通过类名提及基类

```cpp
Manager::Manager( const string& name, const string& ssn, const string& title = "" )
    :Employee(name, ssn), m_title( title ) {
}
```

### 构造函数的更多信息

- 基类总是先构造
- 如果没有显式传递参数给基类
    - 将调用默认构造函数
- 析构函数的调用顺序与构造函数的顺序完全相反

### 继承构造函数

类具有可派生性，派生类自动获得基类的成员变量和接口（虚函数和纯虚函数）

基类的构造函数没有被继承，因此：

```cpp
class A {
public:
    A(int i) {}
};

class B : public A {
public:
    B(int i): A(i), d(i) {} // 透传参数
private:
    int d;
};
```

B的构造函数起到了传递参数给A的构造函数的作用：透传

如果A具有不只一个构造函数，B往往需要设计对应的多个透传

### using声明

派生类用`using`声明来使基类的成员函数成为自己的

- 解决name hiding问题：非虚函数被`using`后成为派生类的函数
- 解决构造函数重载问题

=== "示例1：解决函数重载"
    ```cpp
    class Base {
    public:
        void f(double ) {
            cout << "double\n";
        }
    };
    
    class Derived : Base { //不是public继承
    public:
        using Base::f;
        void f(int ) {
            cout << "int\n";
        }
    };
    
    int main()
    {
        Derived d;
        d.f(4);     // 调用 Derived::f(int)
        d.f(4.5);   // 调用 Base::f(double)
    }
    ```

=== "示例2：继承构造函数"
    ```cpp
    class A {
    public:
        A(int i) { cout << "int\n"; }
        A(double d, int i) {}
        A(float f, char *s) {}
    };
    
    class B : A {
    public:
        using A::A;  // 继承A的构造函数
    };
    
    int main()
    {
        B b(2);  // 调用A(int)构造函数
    }
    ```

!!! note "注意"
    继承构造函数是隐式声明的，如果没有用到就不产生代码

如果基类的函数具有默认参数值，`using`的派生类无法得到默认参数值，就必须转为多个重载的函数

```cpp
class A {
public:
    A(int a=3, double b=2.4) {}
};
```

实际上可以被看作是：
```cpp
A(int, double);
A(int);
A();
```

那么，被`using`之后就会产生相应的多个函数

### Manager成员函数

```cpp
inline void Manager::print( std::ostream& out ) const {
    Employee::print( out ); // call the base class print
    out << m_title << endl;
}

inline const std::string& Manager::get_title() const {
    return m_title;
}

inline const std::string Manager::title_name() const {
    return string( m_title + ": " + m_name ); // access base m_name
}
```

### 使用示例

```cpp
int main () {
    Employee bob( "Bob Jones", "555-44-0000" );
    Manager bill( "Bill Smith", "666-55-1234", "Important Person" );
    
    string name = bill.get_name(); // okay Manager inherits Employee
    //string title = bob.get_title(); // Error -- bob is an Employee!
    
    cout << bill.title_name() << '\n' << endl;
    
    bill.print(cout);
    bob.print(cout);
    bob.print(cout, "Employee:");
    //bill.print(cout, "Employee:"); // Error hidden!
}
```

## 名称隐藏（Name Hiding）

!!! warning "名称隐藏原则"
    如果在派生类中重新定义成员函数，则基类中的所有其他重载函数都不可访问。
    
    我们将在下一次看到关键字`virtual`如何影响函数重载。

## 不被继承的内容

- **构造函数**
  - 合成的构造函数使用成员初始化
  - 在显式复制构造函数中，显式调用基类复制构造函数，否则将调用默认构造函数
- **析构函数**
- **赋值操作**
  - 合成的`operator=`使用成员赋值
  - 显式`operator=`请确保显式调用基类版本的`operator=`
- **私有数据是隐藏的，但仍然存在**

## 访问保护

### 成员

- **Public**: 对所有客户可见
- **Protected**: 对派生自自身的类可见（以及友元）
- **Private**: 仅对自身和友元可见！

### 继承

- **Public**: `class Derived : public Base ...`
- **Protected**: `class Derived : protected Base ...`
- **Private**: `class Derived : private Base ...` (默认)

### 继承如何影响访问

假设类B派生自A，则：

| 基类成员访问指定符 | 继承类型(B是) | public | protected | private |
|-------------------|--------------|--------|-----------|---------|
| public A          | public in B  | protected in B | hidden |
| private A         | private in B | private in B   | hidden |
| protected A       | protected in B | protected in B | hidden |

### 受保护成员的局限性

当派生类行为不当时！

- Protected对所有派生类是公开的
- 因此
    - 使成员函数protected
    - 保持成员变量private

!!! note "本章小结"
    在本章中学习了：

    - 继承的概念和作用
    - 继承的语法和规则
    - 访问控制和权限
    - 构造函数和析构函数在继承中的行为
    - 名称隐藏规则
    - using声明的用法

## pdf资料

<embed src="pdfs/8 Inheritance.pdf" type="application/pdf" width="100%" height="400px" />