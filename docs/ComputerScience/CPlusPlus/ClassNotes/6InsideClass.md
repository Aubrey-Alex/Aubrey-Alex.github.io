# 类的内部机制

!!! abstract "类内部实现机制"
    本章深入探讨C++类的内部实现机制，包括函数重载、委托构造函数、默认参数、内联函数等高级特性，这些是实现高效C++类设计的重要基础。

## 对象与类

<div class="grid" markdown>

=== "对象特性"
    - 表示事物、事件或概念的实例
    - 在运行时响应消息
    - 代表特定实例（例如：这只猫）

=== "类特性"
    - 定义实例的属性和行为
    - 在C++中作为类型存在
    - 定义共同特征（例如：猫这个种类）

</div>

## 函数重载

<div class="grid cards" markdown>

-   :material-function-variant:{ .lg .middle } __基本概念__

    ---
    
    === "重载函数"
        函数重载是指用同一个函数名定义多个功能类似但参数列表不同的函数：
        ```cpp
        void print(char* str, int width);     // #1
        void print(double d, int width);      // #2
        void print(long l, int width);        // #3
        void print(int i, int width);         // #4
        void print(char* str);                // #5
        
        // 调用时根据参数类型自动选择正确的函数
        print("Pancakes", 15);   // 调用 #1
        print("Syrup");          // 调用 #5
        print(1999.0, 10);       // 调用 #2
        print(1999, 12);         // 调用 #4
        print(1999L, 15);        // 调用 #3
        ```
    
    === "自动类型转换"
        当没有完全匹配的函数时，编译器会尝试进行类型转换：
        ```cpp
        void f(short i);
        void f(double d);
        
        f('a');     // char 转为 short
        f(2);       // int 转为 short
        f(2L);      // long 转为 double
        f(3.2);     // 精确匹配 double
        ```
    
    === "const重载"
        可以基于成员函数是否为const进行重载：
        ```cpp
        class MyClass {
        public:
            void f();         // 非const版本
            void f() const;   // const版本
        };
        
        MyClass obj;          // 非const对象
        const MyClass cobj;   // const对象
        
        obj.f();   // 调用非const版本
        cobj.f();  // 调用const版本
        ```

-   :material-arrow-decision-outline:{ .lg .middle } __构造函数重载__

    ---
    
    === "多个构造函数"
        类可以有多个构造函数，用于不同的初始化场景：
        ```cpp
        class Box {
        private:
            double length, width, height;
        public:
            // 默认构造函数
            Box() : length(1.0), width(1.0), height(1.0) {}
            
            // 单参数构造函数
            Box(double side) : length(side), width(side), height(side) {}
            
            // 三参数构造函数
            Box(double l, double w, double h) 
                : length(l), width(w), height(h) {}
        };
        ```
    
    === "重复代码问题"
        多个构造函数可能包含重复代码：
        ```cpp
        class ClassC {
        public:
            int max, min, middle;
            
            ClassC() {}
            
            ClassC(int my_max) {
                max = my_max > 0 ? my_max : 10;
            }
            
            ClassC(int my_max, int my_min) {
                max = my_max > 0 ? my_max : 10;
                min = my_min > 0 && my_min < max ? my_min : 1;
            }
            
            ClassC(int my_max, int my_min, int my_middle) {
                max = my_max > 0 ? my_max : 10;
                min = my_min > 0 && my_min < max ? my_min : 1;
                middle = my_middle < max && my_middle > min 
                       ? my_middle : 5;
            }
        };
        ```

</div>

## 委托构造函数

<div class="grid cards" markdown>

-   :material-source-branch:{ .lg .middle } __基本用法__

    ---
    
    === "委托构造概念"
        委托构造函数(Delegating Constructor)是C++11引入的特性，允许一个构造函数调用同一个类的另一个构造函数：
        ```cpp
        class ClassC {
        public:
            int max, min, middle;
            
            // 基础构造函数
            ClassC(int my_max) {
                max = my_max > 0 ? my_max : 10;
            }
            
            // 委托给第一个构造函数，然后执行自己的代码
            ClassC(int my_max, int my_min) : ClassC(my_max) {
                min = my_min > 0 && my_min < max ? my_min : 1;
            }
            
            // 委托给第二个构造函数，然后执行自己的代码
            ClassC(int my_max, int my_min, int my_middle) 
                : ClassC(my_max, my_min) {
                middle = my_middle < max && my_middle > min 
                       ? my_middle : 5;
            }
        };
        ```
    
    === "执行顺序"
        - 目标构造函数(被委托的构造函数)先执行
        - 委托构造函数后执行
        - 构造函数不能同时使用委托和其他初始化列表
    
    === "链式委托"
        构造函数可以形成委托链：
        ```cpp
        class Info {
        private:
            int type;
            char name;
            
            // 目标构造函数
            Info(int i, char e) : type(i), name(e) {}
            
        public:
            // 委托给私有构造函数
            Info() : Info(1, 'a') {}
            
            // 委托给私有构造函数
            Info(int i) : Info(i, 'a') {}
            
            // 委托给私有构造函数
            Info(char e) : Info(1, e) {}
        };
        ```

-   :material-connection:{ .lg .middle } __委托构造好处__

    ---
    
    === "避免代码重复"
        多个构造函数通常需要执行相似的初始化代码，委托构造避免了这种重复：
        ```cpp
        // 不好的做法：重复代码
        class Widget {
            int x, y, z;
        public:
            Widget() { x = 0; y = 0; z = 0; initRest(); }
            Widget(int a) { x = a; y = 0; z = 0; initRest(); }
            Widget(int a, int b) { x = a; y = b; z = 0; initRest(); }
            
            void initRest() { /* 共同初始化代码 */ }
        };
        
        // 好的做法：委托构造
        class Widget {
            int x, y, z;
        public:
            Widget() : Widget(0, 0, 0) {}
            Widget(int a) : Widget(a, 0, 0) {}
            Widget(int a, int b) : Widget(a, b, 0) {}
            Widget(int a, int b, int c) : x(a), y(b), z(c) {
                // 共同初始化代码
            }
        };
        ```
    
    === "初始化顺序"
        委托构造确保所有构造路径使用相同的初始化顺序：
        ```cpp
        class Info {
        private:
            int type;
            char name;
            
        public:
            // 所有构造函数最终都委托给目标构造函数
            Info() : Info(1) {}
            Info(int i) : Info(i, 'a') {}
            Info(char e) : Info(1, e) {}
            
            // 目标构造函数
            Info(int i, char e) : type(i), name(e) {}
        };
        ```
    
    === "注意事项"
        - 委托构造不能形成循环链（会导致编译错误）
        - 被委托的构造函数的初始化列表和函数体都会被执行

</div>

## 默认参数

<div class="grid" markdown>

=== "基本用法"
    默认参数是在函数声明中给参数提供的默认值，如果调用时没有提供这些参数，编译器会自动使用默认值：
    ```cpp
    // 声明带默认参数的函数
    void print(int width, int height = 10, const char* str = "Default");
    
    // 调用方式
    print(20);             // 使用height和str的默认值
    print(20, 30);         // 使用str的默认值
    print(20, 30, "Test"); // 不使用默认值
    ```

=== "默认参数规则"
    - 默认参数必须从右向左添加（右边的所有参数都必须有默认值）：
    ```cpp
    // 正确
    int harpo(int n, int m = 4, int j = 5);
    
    // 错误：m有默认值但j没有
    int chico(int n, int m = 6, int j);
    
    // 正确：所有参数都有默认值
    int groucho(int k = 1, int m = 2, int n = 3);
    ```
    
    - 默认参数在声明处指定，而不是在定义处：
    ```cpp
    // 头文件中的声明
    void func(int a, int b = 10);
    
    // 源文件中的定义
    void func(int a, int b) {
        // 函数实现
    }
    ```

=== "构造函数中的默认参数"
    默认参数可以用于构造函数，如果所有参数都有默认值，这个构造函数也是默认构造函数：
    ```cpp
    class Box {
    private:
        double length, width, height;
    public:
        // 这是一个带默认参数的构造函数
        // 也是一个默认构造函数
        Box(double l = 1.0, double w = 1.0, double h = 1.0)
            : length(l), width(w), height(h) {}
    };
    
    // 创建Box对象的不同方式
    Box b1;           // 使用所有默认值: (1.0, 1.0, 1.0)
    Box b2(2.0);      // (2.0, 1.0, 1.0)
    Box b3(2.0, 3.0); // (2.0, 3.0, 1.0)
    ```

</div>

## 内联函数

<div class="grid cards" markdown>

-   :material-code-braces:{ .lg .middle } __内联基础__

    ---
    
    === "什么是内联函数"
        内联函数是一种特殊的函数，编译器会尝试在调用处展开函数体，而不是生成函数调用，从而减少函数调用的开销：
        ```cpp
        // 常规函数
        int f(int i) {
            return i * 2;
        }
        
        // 内联函数
        inline int g(int i) {
            return i * 2;
        }
        
        int main() {
            int a = 4;
            int b = f(a);  // 生成函数调用
            int c = g(a);  // 可能被展开为: int c = a * 2;
            return 0;
        }
        ```
    
    === "函数调用开销"

        函数调用涉及多个步骤，会产生开销：
        
        - 压入参数
        - 保存返回地址
        - 跳转到函数地址
        - 为局部变量分配空间
        - 执行函数体
        - 准备返回值
        - 跳回调用点
        - 弹出之前压入的值
    
    === "声明与定义"
        内联函数的声明和定义都应使用`inline`关键字：
        ```cpp
        // 在头文件中声明和定义
        inline int plusOne(int x);
        inline int plusOne(int x) { return ++x; }
        ```

-   :material-swap-horizontal:{ .lg .middle } __内联使用__

    ---
    
    === "内联函数放在头文件中"
        内联函数的定义必须对所有调用点可见，所以通常放在头文件中：
        ```cpp
        // 在头文件中定义内联函数
        // mymath.h
        #ifndef MYMATH_H
        #define MYMATH_H
        
        inline int square(int x) {
            return x * x;
        }
        
        #endif
        ```
        
        - 不必担心多重定义问题，内联函数的定义可以在多个翻译单元中出现
        - 内联只是对编译器的建议，编译器可能不接受这个建议
    
    === "类内定义的成员函数"
        在类定义内部直接定义的成员函数自动成为内联函数：
        ```cpp
        class Point {
        private:
            int x, y;
        public:
            // 自动内联
            int getX() { return x; }
            int getY() { return y; }
            
            // 设置类内成员
            void setX(int new_x);
            void setY(int new_y);
        };
        
        // 类外定义，需要显式指定inline
        inline void Point::setX(int new_x) {
            x = new_x;
        }
        ```
    
    === "内联函数的优缺点"

        优点:

        - 减少函数调用开销，提高性能
        - 编译器可能进行更多优化
        - 比宏更安全（类型检查，作用域规则）
        
        缺点:

        - 增加代码体积
        - 不适合大型或复杂函数
        - 不支持递归
        - 调试时可能更困难

-   :material-sync:{ .lg .middle } __内联变量(C++17)__

    ---
    
    === "内联变量基本概念"
        C++17引入了内联变量，允许在头文件中定义变量而不会导致链接错误：
        ```cpp
        // constants.h
        #ifndef CONSTANTS_H
        #define CONSTANTS_H
        
        inline int globalValue = 42;
        inline const double pi = 3.14159;
        
        #endif
        ```
    
    === "静态成员变量内联"
        使用内联初始化类的静态成员变量：
        ```cpp
        class Config {
        public:
            inline static int maxConnections = 100;
            inline static std::string appName = "MyApp";
        };
        
        // 使用
        std::cout << Config::maxConnections;
        ```
    
    === "inline与weak比较"

        | 特性 | inline | weak |
        |------|--------|------|
        | 主要用途 | 优化函数调用，避免多重定义 | 允许符号覆盖，防止链接冲突 |
        | 适用对象 | 主要用于函数和变量 | 函数、变量、对象 |
        | 符号管理 | 编译器生成函数的多个副本并去重 | 多个弱符号可共存，链接器选择最强的 |
        | 链接行为 | 多个翻译单元定义同一内联函数不会导致错误 | 存在多个弱符号不会导致错误 |
        | 优化重点 | 减少函数调用开销提高性能 | 提供备用实现或插件的可选符号 |

</div>

!!! tip "内联使用建议"
    - **适合内联**：小函数（2-3行）、频繁调用的函数（如循环内部）
    - **不适合内联**：大函数（超过20行）、递归函数、复杂逻辑函数
    - **实用建议**：让编译器决定，使用编译器优化选项，只对关键路径上的小函数使用inline

## pdf资料

<embed src="pdfs/6 Inside Class.pdf" type="application/pdf" width="100%" height="400px" />