## 组合 (Composition)

!!! abstract "组合与命名空间"
    本章深入探讨C++中的组合机制和命名空间，组合是面向对象程序设计中重用实现的重要方式，通过已有对象构建新对象；命名空间则是控制名称作用域、避免命名冲突的机制。

### 组合的基本概念
<div class="grid" markdown>

=== "组合定义"

    - 组合：通过已有对象构建新对象的机制
    - 表达的是"有一个"(has-a)的关系
    - "每个对象的内存由其他对象组成" —— Alan Kay

=== "包含方式"

    对象包含的两种方式：

    - 完全包含 (Fully)
        - 意味着"它是这个对象的一部分"
    - 引用包含 (By reference)
        - 意味着"它在那里"
        - 允许对象之间共享

=== "组合示例"

    Employee对象包含以下元素：

    - 姓名(Name)
    - 地址(Address)
    - 健康计划(Health Plan)
    - 薪资历史(Salary History)：多个Raise对象的集合
    - 主管(Supervisor)：另一个Employee对象
</div>

### 组合实例

<div class="grid cards" markdown>

-   :material-chart-box-outline:{ .lg .middle } __储蓄账户示例__

    ---
    
    === "基本类结构"
    
        ```cpp
        class Person { ... };
        class Currency { ... };
        class SavingsAccount {
        public:
            SavingsAccount(
                const char* name,
                const char* address,
                int cents );
            ~SavingsAccount();
            void print();
        private:
            Person m_saver;
            Currency m_balance;
        };
        ```
    
    === "构造函数实现"
    
        ```cpp
        SavingsAccount::SavingsAccount (
            const char* name,
            const char* address,
            int cents ) : m_saver(name, address),
                        m_balance(0, cents) {}
        ```
    
    === "成员函数实现"
    
        ```cpp
        void SavingsAccount::print() {
            m_saver.print();
            m_balance.print();
        }
        ```
    
-   :material-arrange-bring-forward:{ .lg .middle } __内嵌对象机制__

    ---
    
    === "初始化规则"
    
        - 所有内嵌对象都会被初始化
        - 如果不提供参数且存在默认构造函数，将调用默认构造函数
        - 析构函数将自动调用，顺序与构造相反
        
    === "构造函数的初始化列表"
        
        - 可以用逗号分隔多个对象
        - 为子构造函数提供参数
        - 对象的初始化顺序与类中声明顺序一致，而非初始化列表中的顺序
        
        ```cpp
        class Point {
        private:
            const float x, y;
            Point(float xa = 0.0, float ya = 0.0) : y(ya), x(xa) {}
        };
        ```
    
    === "初始化与赋值的区别"
    
        ```cpp
        // 初始化（在构造函数体执行前）
        Student::Student(string s) : name(s) {}
        
        // 赋值（在构造函数体内）
        Student::Student(string s) { name = s; }
        // string必须有默认构造函数
        ```    

</div>

!!! warning "不使用初始化列表的问题"

    如果不使用初始化列表，而是在构造函数体内使用设置方法：
    
    ```cpp
    SavingsAccount::SavingsAccount (
        const char* name,
        const char* address,
        int cents ) {
        m_saver.set_name(name);
        m_saver.set_address(address);
        m_balance.set_cents(cents);
    }
    ```
    
    问题：
    
    - 会先使用默认构造函数初始化对象
    - 然后在构造函数体内通过赋值修改状态
    - 相当于进行了两次工作，效率较低
    - 对于没有默认构造函数的类型无法使用此方法

### 公有与私有内嵌对象

<div class="grid" markdown>

=== "私有内嵌对象"

    - 通常将内嵌对象设为私有，作为底层实现的一部分
    - 新类只公开内嵌对象的部分功能
    - 实现封装，隐藏内部细节
    
    ```cpp
    class SavingsAccount {
    private:
        Person m_saver;
        Currency m_balance;
    };
    ```

=== "公有内嵌对象"

    - 如果希望子对象的整个公共接口在新对象中可用
    - 内嵌对象的所有公共方法都可被直接访问
    
    ```cpp
    class SavingsAccount {
    public:
        Person m_saver;
        // ...
    }; // 假设Person类有set_name()方法
    
    SavingsAccount account;
    account.m_saver.set_name("Fred");
    ```
</div>

### 完全包含与引用包含
<div class="grid cards" markdown>

-   :material-arrange-bring-to-front:{ .lg .middle } __完全包含__
  
    ---

    - "它在这里，作为这个对象的一部分"
    - 内嵌对象的生命周期与外部对象绑定
    - 构造函数和析构函数会自动调用
    - 对象的内存布局包含所有内嵌对象

    ```cpp
    class SavingsAccount {
    private:
        Person m_saver;      // 完全包含
        Currency m_balance;  // 完全包含
    };
    ```

-   :material-ray-start-arrow:{ .lg .middle } __引用包含__
  
    ---

    - "它在那里"，通过指针或引用访问
    - 内嵌对象与外部对象生命周期可以不同
    - 需要手动初始化和销毁对象

    ```cpp
    class SavingsAccount {
    private:
        Person* m_saver;      // 引用包含
        Currency* m_balance;  // 引用包含
    };
    ```

-   :material-alert-circle-outline:{ .lg .middle } __引用包含应用场景__

    ---

    引用包含通常应用于以下场景：
    
    - 逻辑关系不是完全包含
    - 初始时不知道对象大小
    - 资源需要在运行时分配或连接
    - 需要共享对象
    - 需要动态变化的集合

    其他面向对象语言（如Java）只使用引用包含，而C++两种方式都支持。

</div>

### 模块化与时钟显示实例
<div class="grid" markdown>

=== "模块化概念"

    - 模块化是将整体划分为定义良好的部分的过程
    - 这些部分可以单独构建和检查
    - 并以明确定义的方式交互

=== "时钟显示类图"

    ```
    ClockDisplay ◇--> NumberDisplay
    ```

=== "类实现"

    ```cpp
    // ClockDisplay类实现
    class ClockDisplay {
        NumberDisplay hour;
        NumberDisplay minute;
        // ...
    };
    
    // NumberDisplay类实现
    class NumberDisplay {
        int limit;
        int value;
        // ...
    };
    
    // Clock类实现
    class Clock {
        NumberDisplay hour;
        NumberDisplay minute;
        // ...
    };
    ```
</div>

### 初始化列表详解
<div class="grid" markdown>

=== "基本语法"

    ```cpp
    class Point {
    private:
        const float x, y;
        Point(float xa = 0.0, float ya = 0.0) : y(ya), x(xa) {}
    };
    ```
    
    - 可以初始化任何类型的数据
    - 对于内置类型也进行伪构造函数调用
    - 无需在构造函数体内执行赋值

=== "初始化顺序"

    - 初始化的顺序是按照成员变量声明的顺序，而不是初始化列表中的顺序
    - 析构的顺序与初始化顺序相反
    
    ```cpp
    // 初始化顺序按照x, y的声明顺序
    // 不是按照初始化列表中y, x的顺序
    Point(float xa = 0.0, float ya = 0.0) : y(ya), x(xa) {}
    ```

=== "初始化vs赋值"

    ```cpp
    // 初始化（在构造函数体执行前）
    Student::Student(string s) : name(s) {}
    
    // 赋值（在构造函数体内）
    Student::Student(string s) { name = s; }
    ```
    
    - 初始化发生在构造函数体执行前
    - 赋值发生在构造函数体内
    - 初始化更高效，尤其对于没有默认构造函数的类型
    - const成员和引用成员必须在初始化列表中初始化
</div>

### 命名空间(Namespace)
<div class="grid cards" markdown>

-   :material-folder-multiple-outline:{ .lg .middle } __基本概念__
   
    ---

    === "命名空间的作用"

        - 表达一组类、函数、变量等的逻辑分组
        - 命名空间是一个作用域，类似于类
        - 当只需要名称封装而不需要数据封装时使用
        
        ```cpp
        namespace Math {
            double abs(double);
            double sqrt(double);
            int trunc(double);
        } // 注意：没有结束分号！
        ```

    === "避免命名冲突"

        - 命名空间可以解决全局作用域中的名称冲突：
        
        ```cpp
        // 全局作用域中的命名冲突
        // old1.h
        void f();
        void g();
        
        // old2.h
        void f();
        void g();
        
        // 使用命名空间解决冲突
        // old1.h
        namespace old1 {
            void f();
            void g();
        }
        
        // old2.h
        namespace old2 {
            void f();
            void g();
        }
        ```

    === "定义命名空间"

        - 命名空间通常定义在头文件中：
        
        ```cpp
        // MyLib.h
        namespace MyLib {
            void foo();
            class Cat {
            public:
                void Meow();
            };
        }
        ```
        
        - 实现命名空间中的函数：
        
        ```cpp
        // MyLib.cpp
        #include "MyLib.h"
        
        void MyLib::foo() { cout << "foo\n"; }
        void MyLib::Cat::Meow() { cout << "meow\n"; }
        ```

-   :material-sign-direction:{ .lg .middle } __使用命名空间__
    
    ---

    === "限定名称"

        - 使用作用域解析运算符限定命名空间中的名称：
        
        ```cpp
        #include "MyLib.h"
        
        void main() {
            MyLib::foo();
            MyLib::Cat c;
            c.Meow();
        }
        ```

    === "使用声明(using-declaration)"

        - 引入命名空间中特定名称的本地同义词：
        
        ```cpp
        void main() {
            using MyLib::foo;
            using MyLib::Cat;
            
            foo();  // 等同于MyLib::foo()
            Cat c;  // 等同于MyLib::Cat c
            c.Meow();
        }
        ```
        
        - 在一个地方说明名称来源
        - 消除冗余的作用域限定

    === "使用指令(using-directive)"

        - 使命名空间中的所有名称可用：
        
        ```cpp
        void main() {
            using namespace std;
            using namespace MyLib;
            
            foo();
            Cat c;
            c.Meow();
            cout << "hello" << endl;
        }
        ```

-   :material-merge:{ .lg .middle } __命名空间高级特性__

    ---

    === "名称冲突处理"

        - 使用指令可能创建潜在的歧义：
        
        ```cpp
        // MyLib.h
        namespace XLib {
            void x();
            void y();
        }
        
        namespace YLib {
            void y();
            void z();
        }
        
        void main() {
            using namespace XLib;
            using namespace YLib;
            
            x();        // OK
            y();        // 错误：歧义
            XLib::y();  // OK，解析为XLib
            z();        // OK
        }
        ```
        
        - 使用指令只是使名称可用
        - 歧义只在调用时出现
        - 使用作用域解析符解决冲突

    === "命名空间别名"

        - 为过长或可能冲突的命名空间创建别名：
        
        ```cpp
        namespace supercalifragilistic {
            void f();
        }
        
        namespace short = supercalifragilistic;
        short::f();  // 等同于supercalifragilistic::f()
        ```

    === "命名空间的组合"

        - 从其他命名空间组合新的命名空间：
        
        ```cpp
        namespace first {
            void x();
            void y();
        }
        
        namespace second {
            void y();
            void z();
        }
        
        namespace mine {
            using namespace first;
            using namespace second;
            using first::y;  // 解决冲突，使用first中的y
            void mystuff();
            // ...
        }
        ```

    === "开放式命名空间"

        - 命名空间是开放的，多个声明可以添加到同一个命名空间：
        
        ```cpp
        // header1.h
        namespace X {
            void f();
        }
        
        // header2.h
        namespace X {
            void g();  // X现在拥有f()和g()
        }
        ```
        
</div>

!!! note "本章小结"
    - 组合是一种通过已有对象构建新对象的机制，表达"有一个"关系
    - 组合有两种方式：完全包含和引用包含
    - 初始化列表对于高效构造内嵌对象至关重要
    - 命名空间用于逻辑分组和避免名称冲突
    - 命名空间提供了灵活的名称管理机制，包括别名、组合和选择性引入