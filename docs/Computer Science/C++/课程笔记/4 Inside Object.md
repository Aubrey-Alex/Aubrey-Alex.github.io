# 内部对象

!!! abstract "对象内部机制"
    本章探讨C++对象的内部实现机制，包括访问控制、对象在不同位置的表现、静态成员、引用、常量以及动态内存分配。

## 访问控制

<div class="grid cards" markdown>

-   :material-lock-open:{ .lg .middle } __访问修饰符__

    ---
    
    === "public"
        - 所有成员声明对所有人开放
        - 任何代码都可以访问public成员
        ```cpp
        class X {
        public:
            int i;
            void f();  // 任何地方都可调用f()
        };
        
        X x;
        x.i = 10;     // 可直接访问
        x.f();        // 可直接调用
        ```
    
    === "private"
        - 除了类内部成员函数，其他地方不能访问
        - 实现信息隐藏，保护数据安全
        ```cpp
        class X {
        private:
            int i;
            void f();
        public:
            void g() {
                i = 10;  // 类内可访问
                f();     // 类内可调用
            }
        };
        
        X x;
        x.i = 10;  // 错误！不能访问
        x.f();     // 错误！不能调用
        ```
    
    === "protected"
        - 本类和派生类可见
        - 在继承中使用（后续章节详述）

-   :material-account-group:{ .lg .middle } __友元__

    ---
    
    === "友元函数"
        - 授权非成员函数访问私有成员
        ```cpp
        class X {
        private:
            int i;
        public:
            friend void modify(X& x); // 声明友元函数
        };
        
        void modify(X& x) {
            x.i = 10;  // 可访问私有成员
        }
        ```
    
    === "友元类"
        - 授权整个类访问私有成员
        ```cpp

        class Y; // 前向声明
        
        class X {
        private:
            int i;
        public:
            friend class Y; // Y类是X的友元
        };
        
        class Y {
        public:
            void modify(X& x) {
                x.i = 20; // Y可访问X的私有成员
            }
        };
        ```
    
    === "友元成员函数"
        - 授权另一个类的特定成员函数访问
        ```cpp
        class Y; // 前向声明
        
        class X {
        private:
            int i;
        public:
            friend void Y::f(X* x); // Y::f是X的友元
        };
        ```

</div>

!!! note "struct vs class"
    C++中，struct和class的唯一区别是默认访问权限不同：
    - `struct`默认为`public`（兼容C语言）
    - `class`默认为`private`（强调信息隐藏）
    ```cpp
    struct A { int i; }; // i是public
    class B { int i; };  // i是private
    ```

## 对象位置与生命周期

<div class="grid cards" markdown>

-   :material-toy-brick-outline:{ .lg .middle } __局部对象__

    ---
    
    === "特点"
        - 定义在函数内部
        - 作用域仅限于所在函数或代码块
        - 生命周期从定义点到作用域结束
        ```cpp
        void function() {
            int localVar;        // 局部变量
            ClassName localObj;  // 局部对象
        }  // localVar和localObj在此处销毁
        ```
    
    === "变量分类"
        | 类型 | 作用域 | 生命周期 | 特点 |
        |------|-------|---------|------|
        | 字段(field) | 整个类 | 对象的生命周期 | 使用this指针访问 |
        | 参数(parameter) | 函数内部 | 函数调用期间 | 由调用者初始化 |
        | 局部变量(local variable) | 定义块内 | 块执行期间 | 需手动初始化 |
    
    === "作用域冲突"
        当局部变量与成员变量同名时，局部变量会覆盖成员变量：
        ```cpp
        class X {
            int amount;
        public:
            int refund() {
                int amount;      // 覆盖成员变量
                amount = 100;    // 操作局部变量
                this->amount = 0;// 显式访问成员变量
                return amount;
            }
        };
        ```

-   :material-earth:{ .lg .middle } __全局对象__

    ---
    
    === "特点"
        - 定义在任何函数外部
        - 作用域为整个程序
        - 在程序启动前构造，程序结束时销毁
        ```cpp
        #include "X.h"
        
        X global_x(12, 34);  // 全局对象
        X global_x2(8, 16);  // 另一个全局对象
        
        int main() {
            // global_x和global_x2已构造
            return 0;
        }  // 程序结束，销毁全局对象
        ```
    
    === "构造顺序"
        - 同一文件中的全局对象按声明顺序构造
        - 不同文件中的全局对象构造顺序不确定
        - 析构顺序与构造顺序相反
    
    === "初始化依赖问题"
        当全局对象之间有依赖关系时，可能产生问题：
        ```cpp
        // file1.cpp
        extern Y global_y;
        X global_x(global_y); // 依赖global_y
        
        // file2.cpp
        Y global_y;
        ```
        解决方案：

          - 避免全局对象间依赖
          - 将相关全局对象定义在同一文件中，按正确顺序

</div>

## 静态成员

<div class="grid" markdown>

=== "静态在C++中的含义"
    1. 静态存储：在固定地址上分配，程序整个生命周期存在
    2. 名称可见性限制：内部链接
    
    C++中`static`的使用场景：
    
    | 使用位置 | 含义 |
    |---------|------|
    | 自由函数 | 内部链接(不推荐) |
    | 全局变量 | 内部链接(不推荐) |
    | 局部变量 | 持久存储 |
    | 类成员变量 | 由所有实例共享 |
    | 类成员函数 | 由所有实例共享,只能访问静态成员 |

=== "函数内静态变量"
    在函数内部声明的静态变量，其值在程序的整个生命周期内保持：
    ```cpp
    void counter() {
        static int count = 0;  // 只初始化一次
        count++;               // 保持上次调用后的值
        cout << "Called " << count << " times" << endl;
    }
    ```
    特点：
    - 初始化只发生一次
    - 值在函数调用间保持
    - 作用域仍限于函数内部

=== "类中的静态成员"
    静态成员变量：
    ```cpp
    class Counter {
    private:
        static int count;     // 声明静态成员
    public:
        Counter() { count++; }
        ~Counter() { count--; }
        static int getCount() { return count; }
    };
    
    // 在类外定义并初始化静态成员（必需）
    int Counter::count = 0;   // 不再使用static关键字
    ```
    
    静态成员函数：
    ```cpp
    class X {
    private:
        int i;
        static int count;
    public:
        static void showCount() {
            cout << count << endl;  // 正确：访问静态成员
            // cout << i << endl;   // 错误：不能访问非静态成员
        }
    };
    ```
    
    访问静态成员的方式：
    ```cpp
    // 通过类名访问
    int num = Counter::getCount();
    
    // 通过对象访问
    Counter c;
    int num = c.getCount();
    ```

</div>

## 引用

<div class="grid cards" markdown>

-   :material-link-variant:{ .lg .middle } __引用基础__

    ---
    
    === "定义引用"
        引用是C++中操作对象的新方式，本质上是对象的别名：
        ```cpp
        char c;                // 字符变量
        char *p = &c;          // 指向字符的指针
        char &r = c;           // 字符的引用
        
        int x = 47;
        int &y = x;            // y引用x
        cout << "y = " << y;   // 输出y = 47
        y = 18;                // 修改x的值
        cout << "x = " << x;   // 输出x = 18
        ```
    
    === "引用规则"
        - 引用必须在定义时初始化
        - 初始化建立绑定，之后不能更改
        - 引用的目标必须有位置（左值）
        ```cpp
        int x = 3;
        int &y = x;       // 正确：绑定到x
        const int &z = x; // 正确：可以绑定到const引用
        
        void func(int &r);
        func(i * 3);      // 错误：i*3是右值，没有存储位置
        ```
    
    === "引用与指针比较"
        | 特性 | 引用 | 指针 |
        |------|------|------|
        | 可为空 | 否 | 是 |
        | 依赖现有变量 | 是 | 否 |
        | 可更改指向 | 否 | 是 |
        | 使用方式 | 直接使用 | 需解引用 |
        | 初始化 | 必须初始化 | 可以不初始化 |

-   :material-arrow-right-bold:{ .lg .middle } __右值引用__

    ---
    
    === "概念"
        C++11引入的特性，可以绑定到临时对象（右值）：
        ```cpp
        int x = 20;           // 左值
        int&& rx = x * 2;     // 右值引用，延长x*2的生命周期
        int y = rx + 2;       // 可以复用：值为42
        
        rx = 100;             // 一旦初始化，右值引用变成左值
                              // 可以被赋值
        
        int&& rrx1 = x;       // 错误：右值引用不能绑定左值
        const int&& rrx2 = x; // 错误：同上
        ```
    
    === "函数重载"
        可以基于参数是左值还是右值进行重载：
        ```cpp
        // 接受左值
        void fun(int& lref) {
            cout << "l-value" << endl;
        }
        
        // 接受右值
        void fun(int&& rref) {
            cout << "r-value" << endl;
        }
        
        int main() {
            int x = 10;
            fun(x);    // 输出：l-value reference
            fun(10);   // 输出：r-value reference
        }
        ```
    
    === "const引用"
        常量引用可以绑定右值：
        ```cpp
        void fun(const int& clref) {
            cout << "l-value const reference" << endl;
        }
        
        int main() {
            fun(10);  // 常量引用可接受右值
        }
        ```

</div>

## 常量

<div class="grid cards" markdown>

-   :material-shield-lock:{ .lg .middle } __常量基础__

    ---
    
    === "常量定义"
        常量是不可修改的变量：
        ```cpp
        const int x = 123;  // 定义常量
        x = 27;             // 错误：不能修改
        x++;                // 错误：不能修改
        
        int y = x;          // 正确：可以复制到非常量
        y = x;              // 正确：同上
        
        const int z = y;    // 正确：非常量可以初始化常量
        ```
    
    === "常量特点"
        - 常量遵循作用域规则
        - C++中的const默认为内部链接
        - 编译器尽量避免为const分配存储空间
        - 使用`extern`可强制分配存储空间
    
    === "编译时常量与运行时常量"
        编译时常量可用于需要编译时确定值的场合：
        ```cpp
        const int bufsize = 1024;
        int finalGrade[bufsize];  // 正确：编译时常量
        
        int x;
        cin >> x;
        const int size = x;       // 运行时常量
        double avg[size];         // 错误：需要编译时常量
        ```

-   :material-format-color-text:{ .lg .middle } __指针与常量__

    ---
    
    === "常量指针和指向常量的指针"
        两种不同的概念：
        ```cpp
        // 指向常量的指针：指针指向的内容不能修改
        const char *p = "ABCD";  // (*p)是常量
        *p = 'b';                // 错误：不能修改*p
        p++;                     // 正确：p可以改变
        
        // 常量指针：指针本身不能修改
        char * const q = "abc";  // q是常量
        *q = 'c';                // 正确：*q可以改变
        q++;                     // 错误：q不能改变
        ```
    
    === "组合使用"
        解读复杂声明的技巧：
        ```cpp
        const string* p;      // p指向常量string
        string const* p;      // 同上
        string* const p;      // p是指向string的常量指针
        const string* const p;// p是指向常量string的常量指针
        ```
    
    === "常量转换规则"
        - 非常量可以转为常量（安全）
        - 常量不能转为非常量（除非使用`const_cast`）
        ```cpp
        void f(const int* x);
        
        int a = 15;
        f(&a);              // 正确：非常量转常量
        
        const int b = a;
        f(&b);              // 正确
        
        void g(int* x);
        g(&b);              // 错误：常量不能转非常量
        ```

-   :material-shield-check:{ .lg .middle } __常量与类__

    ---
    
    === "常量成员函数"
        不能修改类的状态：
        ```cpp
        class Date {
            int day, month, year;
        public:
            int get_day() const {
                day++;               // 错误：修改了成员
                set_day(12);         // 错误：调用非常量成员
                return day;          // 正确：只读访问
            }
            
            void set_day(int d) {
                day = d;             // 正确：非常量成员
            }
        };
        ```
    
    === "常量对象"
        只能调用常量成员函数：
        ```cpp
        // 非常量对象
        Date when(1, 1, 2001);
        int day = when.get_day();  // 正确
        when.set_day(13);          // 正确
        
        // 常量对象
        const Date birthday(12, 25, 1994);
        day = birthday.get_day();  // 正确
        birthday.set_day(14);      // 错误：不能调用非常量成员函数
        ```
    
    === "类中的常量"
        ```cpp
        class HasArray {
            // 方法1：使用enum（C++98技巧）
            enum { size = 100 };
            int array1[size];  // 正确
            
            // 方法2：静态常量（C++11支持）
            static const int length = 100;
            int array2[length];  // 正确
            
            // 错误写法
            const int size2;     // 需要在构造函数初始化
            int array3[size2];   // 错误：非编译时常量
        };
        ```

</div>

## 动态内存分配

<div class="grid" markdown>

=== "new和delete操作符"
    动态分配内存是C++的重要特性：
    ```cpp
    // 分配单个对象
    int* p = new int;           // 分配一个int
    *p = 10;                    // 使用分配的内存
    delete p;                   // 释放内存
    
    // 分配对象并初始化
    int* q = new int(42);       // 分配并初始化为42
    Student* s = new Student(); // 调用默认构造函数
    delete s;                   // 调用析构函数并释放内存
    
    // C++11初始化语法
    int* r = new int{42};       // 使用统一初始化语法
    ```

=== "动态数组"
    ```cpp
    // 分配数组
    int* arr = new int[10];     // 分配10个int的数组
    
    // 使用数组
    for(int i = 0; i < 10; i++) {
        arr[i] = i * 10;
    }
    
    // 释放数组（注意使用delete[]）
    delete[] arr;               // 释放整个数组
    
    // C++11初始化数组
    int* nums = new int[3]{1, 2, 3}; // 初始化数组
    delete[] nums;
    ```

=== "使用建议"
    使用new/delete的注意事项：

    1. 不要用delete释放非new分配的内存
    2. 不要重复释放同一块内存
    3. 用new[ ]分配的数组必须用delete[ ]释放
    4. 用new分配的单个对象必须用delete释放
    5. 对空指针使用delete是安全的（无操作）
    
    ```cpp
    // 错误示例
    int* p = new int;
    int* a = new int[10];
    
    a++;                 // 指针移动后
    delete[] a;          // 错误：释放的不是数组起始位置
    
    Student* r = new Student[10];
    delete r;            // 错误：应使用delete[]
    
    Student s;
    delete &s;           // 错误：不是new分配的
    ```

</div>

!!! tip "智能指针"
    C++11引入了智能指针，可以自动管理内存，避免内存泄漏：
    ```cpp
    #include <memory>
    
    // unique_ptr：独占所有权
    std::unique_ptr<int> p1(new int(42));
    
    // shared_ptr：共享所有权
    std::shared_ptr<int> p2 = std::make_shared<int>(100);
    std::shared_ptr<int> p3 = p2;  // p2和p3共享所有权
    
    // 离开作用域时自动释放内存
    ```