# 定义类

!!! abstract "类的概念"
    类是C++中实现面向对象编程的基本单位，它将数据和操作数据的方法封装在一起。一个类既包含属性(数据成员)，也包含服务(成员函数)。

## 从结构体到类

<div class="grid cards" markdown>

-   :material-arrow-right-bold-box:{ .lg .middle } __C语言的结构体__

    ---
    
    === "结构体定义"
        ```c
        // C语言中的点结构
        typedef struct point {
            float x;
            float y;
        } Point;
        
        // 使用结构体
        Point a;
        a.x = 1;
        a.y = 2;
        ```
    
    === "相关函数"
        ```c
        // 打印函数
        void print(Point* p) {
            printf("%d %d\n", p->x, p->y);
        }
        
        // 移动函数
        void move(Point* p, int dx, int dy) {
            p->x += dx;
            p->y += dy;
        }
        
        // 使用函数
        Point a, b;
        a.x = b.x = 1;
        a.y = b.y = 1;
        move(&a, 2, 2);
        print(&a);
        print(&b);
        ```

-   :material-arrow-right-bold-box-outline:{ .lg .middle } __C++的类实现__

    ---
    
    === "类定义"
        ```cpp
        // C++中的点类
        struct Point {
            void init(int x, int y);
            void move(int dx, int dy);
            void print();
            int x;
            int y;
        };
        ```
    
    === "成员函数实现"
        ```cpp
        // 初始化函数
        void Point::init(int ix, int iy) {
            x = ix; 
            y = iy;
        }
        
        // 移动函数
        void Point::move(int dx, int dy) {
            x += dx; 
            y += dy;
        }
        
        // 打印函数
        void Point::print() {
            cout << x << ' ' << y << endl;
        }
        ```
    
    === "使用方法"
        ```cpp
        Point p;
        p.init(1, 2);
        p.move(10, 10);
        p.print();
        ```

</div>

## 类的内部机制

<div class="grid" markdown>

=== "this指针"
    - `this`是所有成员函数的隐藏参数，类型为当前类的指针
    - 调用成员函数时，对象地址会自动传给this指针
    ```cpp
    // 成员函数声明
    void Point::move(int dx, int dy);
    
    // 实际上可理解为
    void Point::move(Point* this, int dx, int dy);
    
    // 调用函数
    p.move(10, 10);
    
    // 实际上可理解为
    Point::move(&p, 10, 10);
    ```

=== "作用域解析符::"
    - `::`用于指定函数属于哪个类
    - `<类名>::<函数名>`表示该函数是类的成员
    ```cpp
    // 全局函数
    void f() { /*...*/ }
    
    // S类的成员函数
    void S::f() {
        ::f();      // 调用全局的f()
        ::a++;      // 全局变量a
        a--;        // 类成员变量a
    }
    ```

=== "成员函数调用"
    - 在成员函数内调用其他成员函数时，不需要指定对象
    - 可以使用`this->`显式指明，但通常不必要
    ```cpp
    void Point::move_and_print(int dx, int dy) {
        move(dx, dy);    // 直接调用
        print();         // 直接调用
        
        // 也可以写成
        this->move(dx, dy);
        this->print();
    }
    ```

</div>

## 面向对象的思想

<div class="grid cards" markdown>

-   :material-ticket-outline:{ .lg .middle } __案例：售票机__

    ---
    
    === "需求描述"
        售票机在客户投入正确金额后打印票据。客户"投入"钱，然后请求打印票据。机器会记录收集的总金额。
    
    === "面向过程思维"
        1. 走到机器前
        2. 向机器投币
        3. 机器打印票据
        4. 拿票离开
        
        这种方法模拟了买票的过程，但没有创建真正的"机器"对象。
    
    === "面向对象思维"
        将售票机作为一个对象，具有属性和操作。
        ```cpp
        class TicketMachine {
            // 数据：属性或状态
            int PRICE;      // 票价
            int balance;    // 当前余额
            int total;      // 总收入
            
            // 操作：函数
            void showPrompt();  // 显示提示信息
            void getMoney();    // 接收金钱
            void printTicket(); // 打印票据
            void showBalance(); // 显示余额
            void printError();  // 打印错误
        };
        ```

-   :material-code-braces:{ .lg .middle } __类的声明与定义__

    ---
    
    === "文件组织"
        - 类声明和成员函数原型放在头文件(.h)中
        - 成员函数定义放在源文件(.cpp)中
        ```
        // Point.h
        class Point {
        public:
            void init(int x, int y);
            void print() const;
            void move(int dx, int dy);
        private:
            int x;
            int y;
        };
        
        // Point.cpp
        #include "Point.h"
        
        void Point::init(int ix, int iy) {
            x = ix; y = iy;
        }
        // 其他成员函数实现...
        ```
    
    === "头文件规范"
        1. 每个头文件对应一个类声明
        2. 与同名源文件关联
        3. 使用预处理器防止重复包含
        ```cpp
        #ifndef POINT_H
        #define POINT_H
        
        // 类声明...
        
        #endif // POINT_H
        ```
    
    === "编译单元"
        - 编译器一次只处理一个.cpp文件，生成.obj文件
        - 链接器将所有.obj文件链接成一个可执行文件
        - 头文件提供了跨编译单元的信息共享

</div>

## 构造函数与析构函数

<div class="grid cards" markdown>

-   :material-hammer-wrench:{ .lg .middle } __构造函数__

    ---
    
    === "基本概念"
        - 构造函数是与类同名的特殊成员函数
        - 在对象创建时自动调用，确保初始化
        - 可以有参数，也可以没有参数
        ```cpp
        class Point {
        public:
            Point(int x, int y);  // 构造函数
            // ...
        private:
            int x, y;
        };
        
        // 定义与使用
        Point::Point(int xx, int yy) {
            x = xx;
            y = yy;
        }
        
        Point p(10, 20);  // 创建对象并初始化
        ```
    
    === "初始化列表"
        - 成员变量可以在声明处初始化
        - 也可以在构造函数的初始化列表中初始化
        ```cpp
        // 在声明处初始化
        class Point {
            int x = 0;  // 默认值为0
            int y = 0;
        };
        
        // 使用初始化列表
        Point::Point(int xx, int yy) 
            : x(xx), y(yy)  // 初始化列表
        {
            // 函数体可以为空
        }
        ```
    
    === "默认构造函数"
        - 不需要参数的构造函数称为默认构造函数
        - 如果没有定义任何构造函数，编译器会自动创建一个
        ```cpp
        class NoConstructor {
            int i;  // 未初始化
        };
        // 编译器自动创建默认构造函数
        
        class HasConstructor {
        public:
            HasConstructor(int a);  // 只有带参数的构造函数
            int i;
        };
        // 没有默认构造函数
        
        HasConstructor h;  // 错误！没有默认构造函数
        ```

-   :material-delete:{ .lg .middle } __析构函数__

    ---
    
    === "基本概念"
        - 析构函数是与类同名但前面有波浪号(~)的特殊成员函数
        - 在对象销毁时自动调用，确保清理工作
        - 从不带参数，也不能重载
        ```cpp
        class X {
        public:
            ~X();  // 析构函数
            // ...
        };
        ```
    
    === "调用时机"
        析构函数在以下情况自动调用：
        
          - 对象离开作用域时
          - 动态分配的对象被删除时
          - 程序结束时全局对象
        ```cpp
        {
            Point p(1, 2);  // 构造函数调用
            // ...
        }  // 作用域结束，析构函数调用
        ```
    
    === "存储分配与初始化"
        - 编译器在作用域开始处分配所有存储空间
        - 构造函数在对象定义处执行，而非作用域开始处
        ```cpp
        {  // 作用域开始，分配空间
            // ...
            Point p(1, 2);  // 此处调用构造函数
            // ...
        }  // 作用域结束，调用析构函数
        ```

</div>

!!! tip "聚合初始化"
    C++支持聚合初始化，可以一次性初始化多个成员：
    ```cpp
    // 数组初始化
    int a[5] = {1, 2, 3, 4, 5};  // 完全初始化
    int b[6] = {5};              // b[0]=5, 其余为0
    int c[] = {1, 2, 3, 4};      // 大小由初始化器决定
    
    // 结构体初始化
    struct X {
        int i;
        float f;
        char c;
    };
    
    X x1 = {1, 2.2, 'c'};        // 按顺序初始化
    X x2[3] = {{1, 1.1, 'a'}, {2, 2.2, 'b'}};  // 数组的初始化
    
    // 有构造函数的类也可以使用聚合初始化
    struct Y {
        float f;
        int i;
        Y(int a);  // 构造函数
    };
    
    Y y1[] = {Y(1), Y(2), Y(3)};  // 使用构造函数初始化
    ```

## pdf资料

<embed src="pdfs/3 Defining Class.pdf" type="application/pdf" width="100%" height="400px" />