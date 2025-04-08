# 内部对象 II

!!! abstract "对象内部机制进阶"
    本章深入探讨C++对象的高级内部机制，重点关注引用、常量及动态内存分配。这些概念是高效C++编程的基础。

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
        - 引用必须在定义时用变量初始化
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

-   :material-arrow-right-bold:{ .lg .middle } __左值与右值__

    ---
    
    === "基本概念"
        - **左值**：可出现在赋值表达式左侧，有地址、有名字
          - 包括变量、引用
          - 解引用、数组下标、点操作符、箭头操作符的结果
        
        - **右值**：只能出现在赋值表达式右侧，无地址或无名
          - 包括字面量、表达式的临时结果
        
        ```cpp
        int a = 10;      // a是左值，10是右值
        int& ref = a;    // ref是左值引用，绑定到左值a
        ```
    
    === "C++11中的右值分类"
        C++11中，右值分为两类：
        
        1. **纯右值(prvalue)**：传统C++中的右值概念
           - 临时变量（如函数返回的非引用值）
           - 运算表达式（如1+3）产生的值
           - 字面量（如2、'c'、true）
           - 类型转换结果、lambda表达式
        
        2. **将亡值(xvalue)**：与右值引用相关的表达式
           - 将要被移动的对象
           - 返回右值引用T&&的函数返回值
           - std::move的返回值
    
    === "右值引用"
        C++11引入的右值引用能绑定到右值（临时对象）：
        ```cpp
        int x = 20;           // 左值
        int&& rx = x * 2;     // 右值引用，延长x*2的生命周期
        int y = rx + 2;       // 可以复用：值为42
        
        rx = 100;             // 一旦初始化，右值引用变成左值
                              // 可以被赋值
        
        int&& rrx1 = x;       // 错误：右值引用不能绑定左值
        const int&& rrx2 = x; // 错误：同上
        ```

-   :material-transit-transfer:{ .lg .middle } __右值引用应用__

    ---
    
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
            fun(x);    // 输出：l-value
            fun(10);   // 输出：r-value
        }
        ```
    
    === "右值引用生命期延长"
        右值引用可以延长临时对象的生命周期：
        ```cpp
        T&& a = ReturnRvalue();
        ```

        - ReturnRvalue函数返回的右值通常在表达式结束后生命终结
        - 通过右值引用，该右值"重获新生"
        - 右值生命期将与引用变量a的生命期一样
        - 比普通拷贝少一次构造和析构开销
    
    === "std::move"
        将左值转换为右值引用，用于支持移动语义：
        ```cpp
        #include <utility>
        
        std::vector<int> createVector() {
            std::vector<int> result = {1, 2, 3, 4, 5};
            return std::move(result);  // 显式移动，避免拷贝
        }
        
        // 在移动构造函数中使用
        MyClass(MyClass&& other) noexcept 
            : resource(std::move(other.resource)) {
            other.resource = nullptr;  // 资源已转移，源对象置空
        }
        ```
        
        **注意**：移动操作会改变源对象的状态，使用后源对象通常处于"有效但未指定"状态

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
        double avg[size];         // 在C++11中正确，早期版本错误
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
        string p1("Fred");      // 普通string
        
        const string* p = &p1;  // p指向常量string
        string const* p = &p1;  // 同上，两种写法等价
        string* const p = &p1;  // p是指向string的常量指针
        ```
    
    === "字符串字面量"
        字符串字面量是常量：
        ```cpp
        char* s = "Hello, world!";  // 警告：应为const char*
        // s实际是const char*，但编译器允许不写const
        *s = 'h';  // 未定义行为！不应修改
        
        // 如需修改，应使用数组
        char s[] = "Hello, world!";  // 可修改的字符数组
        s[0] = 'h';  // 正确
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
            
            // 常量成员变量需要在构造函数初始化列表中初始化
            const int size2;
            HasArray(int s) : size2(s) {}
            
            // 错误写法
            int array3[size2];   // 错误：非编译时常量
        };
        ```

-   :material-function-variant:{ .lg .middle } __函数与常量__

    ---
    
    === "常量参数"
        对于值传递的参数，const通常没有太大意义：
        ```cpp
        void f1(const int i) {
            i++;    // 错误：不能修改常量
        }
        ```
    
    === "常量返回值"
        对于值返回，const通常也没有太大意义：
        ```cpp
        int f3() { return 1; }
        const int f4() { return 1; }
        
        int main() {
            const int j = f3();  // 正确：可以赋给常量
            int k = f4();        // 正确：常量可赋给非常量
        }
        ```
    
    === "传递和返回地址"
        当传递指针或引用时，const很重要：
        ```cpp
        // 推荐：参数声明为const引用或指针
        void display(const MyClass& obj);   // 不会修改obj
        void process(const int* data);      // 不会修改*data
        
        // 可能修改对象
        void update(MyClass& obj);          // 可能修改obj
        void modify(int* data);             // 可能修改*data
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

!!! tip "现代C++内存管理"
    在现代C++（C++11及以后）中，建议使用智能指针而非裸指针管理动态内存：
    
    ```cpp
    #include <memory>
    
    // unique_ptr：独占所有权
    std::unique_ptr<int> p1(new int(42));
    auto p2 = std::make_unique<int>(100);  // C++14
    
    // shared_ptr：共享所有权
    std::shared_ptr<int> p3 = std::make_shared<int>(100);
    std::shared_ptr<int> p4 = p3;  // p3和p4共享所有权
    
    // 使用时无需手动释放内存
    ```
    
    智能指针能够自动管理内存生命周期，有效避免内存泄漏和悬挂指针等问题。