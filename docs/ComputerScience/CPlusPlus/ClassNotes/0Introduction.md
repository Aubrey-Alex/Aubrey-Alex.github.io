# C++ 简介

!!! abstract "什么是C++"
    C++是一门高效、灵活的编程语言，由Bjarne Stroustrup于1980年代初设计开发。它在C语言的基础上添加了面向对象编程的支持，同时保持了C语言的高效性和灵活性。

<div class="grid cards" markdown>

-   :material-language-cpp:{ .lg .middle } __C语言特点__

    ---
    
    === "优势"
        - 程序运行高效
        - 可直接访问硬件，适合操作系统和嵌入式系统开发
        - 语言灵活性强
    
    === "局限"
        - 类型检查不够严格
        - 对大型程序支持不足
        - 仅支持面向过程编程

-   :material-plus-circle:{ .lg .middle } __C++改进__

    ---
    
    === "核心特性"
        - 数据抽象
        - 访问控制
        - 初始化与清理
        - 引用
        - 函数重载
    
    === "高级特性"
        - 流式输入输出
        - 名称控制
        - 运算符重载
        - 更安全和强大的内存管理
        - 模板
        - 异常处理

</div>

### 编程范式支持

<div class="grid" markdown>

=== "面向过程编程"
    传统的C语言编程方式，注重步骤的实现

=== "面向对象编程"
    支持封装、继承、多态等特性

=== "泛型编程"
    通过模板实现代码复用

</div>

### 重要概念

<div class="grid cards" markdown>

-   :material-cube-outline:{ .lg .middle } __面向对象核心__

    ---
    
    - 封装（Encapsulation）
    - 继承（Inheritance）
    - 多态（Polymorphism）
    - 覆盖（Overriding）
    - 接口（Interface）

-   :material-cog-outline:{ .lg .middle } __设计原则__

    ---
    
    - 内聚（Cohesion）
    - 耦合（Coupling）
    - 容器类（Collection Classes）
    - 模板（Template）
    - 责任驱动设计（Responsibility-driven Design）

</div>

### C++输入输出

<div class="grid cards" markdown>

-   :material-console:{ .lg .middle } __基本输出__

    ---
    
    ```cpp
    #include <iostream>
    using namespace std;
    
    int main() {
        cout << "Hello, World! I am " << 18 << " Today!" << endl;
        return 0;
    }
    ```
    
    - `iostream`: 包含输入输出功能
    - `cout`: 控制台输出对象
    - `<<`: 输出运算符
    - `endl`: 结束当前行并刷新缓冲区

-   :material-keyboard:{ .lg .middle } __基本输入__

    ---
    
    ```cpp
    #include <iostream>
    using namespace std;
    
    int main() {
        int number;
        cout << "Enter a decimal number: ";
        cin >> number;
        cout << "The number you entered is " << number << endl;
        return 0;
    }
    ```
    
    - `cin`: 控制台输入对象
    - `>>`: 输入运算符

</div>

!!! info "发展历程"
    - 1978：Stroustrup在剑桥大学使用Simula进行仿真编程
    - 1979：在AT&T实验室开发"C with Classes"
    - 1980：实现了大部分C++特性，但还没有虚函数
    - 1983：加入虚函数特性，命名为C++（由Rick Mascitti提出）
    - 1985：发布Cfront和第一版《The C++ Programming Language》
    - 1990：成立ANSI C++委员会
    - 1998：ISO/IEC 14882标准发布

!!! tip "C++与C的关系"
    虽然C++兼容大部分C语言特性（C++ => C=C+1），但它是一个独立的混合语言，提供了更多的编程特性和更强大的抽象能力。建议将C++视为一个独立的语言来学习，而不是C语言的简单扩展。

!!! note "C++参考书籍"
    - C++ Prime
    - Thinking In C++, Ver. 2, Vol. 1 & 2
    - The C++ Programming Language
    - C++: The Core Language
    - Essential C++
    - Effective C++
    - Inside the C++ Object Model
    - C++ Templates

## pdf资料

<embed src="pdfs/0 Introduction.pdf" type="application/pdf" width="100%" height="400px" />